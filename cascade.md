There are two groups of people who argue a lot about styling: the CSS files people and the Tailwind users. Every now and then, some blog posts hit pieces are written to try to convince the other side that they are wrong and should see the light. 
This post is not one of those; I want to showcase the shortcomings and wins of both sides and explain why I'm personally looking for a more middle-ground approach.

## Biases

Before we start, I would like to disclose my biases to provide some context for the post:

 - I mainly write CSS in the context of design systems. As an application developer, I really don't want to have to make micro-decisions about styling on every element.
 - I like CSS; I studied CSS Zen Garden (Kyoto Gardens being the clearly superior theme) back in the day, and I believe it's a fantastic language.
 - I never found a home in BEM. Just like Tailwind, it deprives you of the cascade/specificity.
 - I appreciate what TypeScript has done for the JavaScript ecosystem. I like types.
 - I'm a little burned out by refactoring styles from one system to another or living in hybrid-styled products that implement styles in a few different ways.
 - Stitches is currently my preferred way of styling—mostly there, but not maintained anymore.
 - I created a proof of concept of a statically extracted styling library.

## Benchmarks

The elephant in the room is benchmarks. I will explore this aspect in a follow-up post, as it's a difficult topic and we should always take one-dimensional or micro-benchmarks with a grain of salt.
For now, I would like to ask you what threshold you consider relevant for performance differences. What kind of performance are you looking for (at compile time, initial load, re-paints)? 
Make sure to mark those numbers somewhere, like in the comments (hi, engagement bait), and we'll revisit this with data at a later time. I would like you to come with an open mind and hear my arguments without the "utility is always faster" or "CSS files are the only way" mindsets. In the search for greatness, sometimes we may forget what is actually good.

## Setting the Table

One last stop before we start: I would like to define a few things to ensure we are speaking about the same concepts:

 - CSS files: This includes any sort of manual CSS assignment, straight .css files, SASS, SCSS, Stylus, CSS modules (controversial pick right there).
 - Atomic libraries: Any library that relies on utility class names, where the style composition happens in the HTML. Think of Tailwind, Panda, Stylex, etc.
 - Runtime CSS-in-JS: Any styling libraries that generate stylesheets on the client. If these libraries allow for runtime string interpolation, I will refer to them as Satan.
 - Statically extracted: CSS-in-JS where the styles are generated at build time. Once generated, it is virtually equivalent to CSS files.
 - CSS-in-TS: Essentially CSS-in-JS but with type safety as a first-class citizen, whether it's at runtime or build time.

## CSS Files and its Bear Traps

CSS files are my first love. I used CSS, SASS, and then CSS modules (SCSS modules, actually) pretty exclusively early in my career. Life was good and simple.
Why would a CSS enthusiast like myself be pushed to CSS-in-JS? 

### Let's make some enemies (1/3).
As soon as the projects I worked on became bigger, involved more "legacy code," or just had more contributors, some serious bear traps started to appear everywhere.

People will immediately mention the lack of collocation of styles and markup as the most problematic drawback of CSS files. I can see this argument and mostly agree that it improves the developer experience significantly, but my main concerns are more about safety, intent communication, DRY, and maintenance.

### Safety

The main weakness of CSS, to me, is that everything is matched against strings. Whenever you get into CSS land, it feels like you are downgrading from TypeScript to JavaScript. 
Unless you use a battery of tools, you get no LSP (Language Server Protocol) telling you that this class does not exist or that the CSS variable token has a typo. When no errors are thrown, you have to rely on your eyes, manual testing, and the style inspector. 
CSS-in-TS helps you here; just make sure you have a proper library, and you'll never have to worry about this whole class of mistakes. This is the reason why I also prefer the object notation (e.g., backgroundColor: 'blue') over the CSS string-based libraries. 
You can then rely on TypeScript introspection to ensure all of your styles are sound, with no external tools required (assuming you are already using TypeScript; more on that later).

### Intent Communication

Once you reach a certain threshold, it becomes very difficult to know what has been designed to be reused, what requires a copy-paste, or what can be a simple additional selector added to a rule. By extension, it makes it difficult to know where things are used and the blast radius of a change.

### DRY

As a corollary of the previous point, having access to the whole CSS file declaration, people tend to be tempted to avoid repeating themselves. To save a few bytes, styles become more and more complex, not because the styling needs to be more complex, but because we introduce a lot of style coupling rather than simply copying and pasting. There is a way to achieve this properly; more on that later.

### Maintenance

CSS is very freeform, and that's a good thing. You can build it as complex as you want or need. But at some point, a new contributor shows up and, either due to a lack of context, knowledge, skill, or time, starts meticulously breaking every rule you have set, and the tug-of-war begins. If you've worked at a place with multiple contributors, you've most likely seen stuff like:

 - .wrapper.wrapper.wrapper {} to artificially boost specificity
 - A few !important declarations here and there
 - Classes impacting children 4, 5, 6, or more nesting levels below (or worst, no direct nesting relations specified).
 - People relying on unrelated parts of the styles (think a #root div {} rule). Refactoring those less specific rules always comes with a fair share of breaking unrelated styles.
 - A bunch of deprecated classes that no one removes because it's scary and might break something completely unrelated.

If the only way to apply styles is to stand up the application and visually check that nothing else applies to your elements in the inspect tool, your styling solution is not working for you; you are working against it. 
Unless you have a full visual regression test suite or a very small amount of styles, you are most likely using CSS as an append-only ledger; you add code to the bottom of the file and pray you didn't break anything.
In this world of CSS-in-JS/TS, eliminating dead code is the same as in the rest of your application: find unused exports and cull them. If you overdo it, the build breaks. This approach is far more welcoming for new contributors and helps prevent the growing layers of eras of styling in a project.

## Previous gen CSS-in-JS

Ok, it's now time to move on to the trendy styling library of the days, part of it was curiosity, part of it was buying into the big developer experience wins. If you need some styles, just declare a new component, and you're off to the race.
I start to encounter a few hard walls quite quickly. I'm now convinced runtime libraries promote bad practices and should be avoided (let's make ennemies 2/3)

### Performance

https://x.com/sebastienlorber/status/1828846945193849059?t=1YXI1Gp86HcJ1ydFJWl_xg&s=09

Using react and buying into the whole re-rendering is fine mindset. Runtime libraries need to recompute styles all the time. Modifying the color prop on the element, let's blow the whole class up and regenerate a new one on the fly during the render loop. More styled components means more unnecessary calculation. We'll see with the benchmark how bad it is, but my experience tends to suggest this is the only period of my dev career that I had to worry about styling performance.

### Un-necessary DOM nesting
Now that we have a Wrapper component, let's import that everywhere and wrap everything because HTML nodes are free, right?
I've seen <Text> components as a series of nested components where each nesting level was hypernormalized in the name of composability (think <Wrapper><Flex><Alert>). In a way, this is recreating the atomic paradigm with extra HTML nodes.

### Loosing touch with the HTML
Somewhat related to the previous point. When you start creating those generic components, the HTML is slowly sidestepping away. 
This is how you get divs inside buttons that are within spans. It technically works but creates a mess. You might not care because *it works*, but I do.

### Extending styles is rough
Do we really need class inheritance in our CSS?
const SuperWrapper = styled(Wrapper)``. Not a fan.

### Theming is rough
Creating themes is defined via a kind of global object. It works well in small situations (even if not the most performant). What always ends up happening is you start using part of the theme as props to respond to different logic. If this is a primary button, pass the theme.primary.text as the color prop to the component. This means runtime evaluation of the styles (performance again), this means wrapping your parent component in a withTheme HoC (performance once again) and leaking styles into the logic.
There's a way to solve this if you are really strict about it, like enforcing generic props to drive variance (more on that later). That would be setting a prop as role: 'Primary' | 'Secondary' | 'neutral' and do the theme switching in the style declaration. In practice people will take the shortcut and

### Type safety is rough
Most of those libraries rely on manually annotating the allowed styles styled.div<{role: 'Primary' | 'Secondary' | 'neutral'}>`...`, This once again technically works but is an extra step that really doesn't have to exist (the function has all the information it needs to infer the types), and in practice gets skipped quite often and you are back with a big `any` on the props.

### Debugging is rough
As someone that often look at the raw HTML and jump between section of the markup there, the barrage of generated class names makes my life so much harder.
Those class names are also difficult to trace back in breadcrumbs bug reports like sentry, user clicked on `div.kj98 > button.ga45.ew84`, cool cool cool.
Same for targeting element in E2E test environment, not that you should use those, but it's a nice to have available in some situations.

## Jumping on the tailwind boat

A lot of the success of tailwind I believe is due to the lack of good alternative to author styles nicely with limited chances to burn yourselves. For all application development purposes, Tailwind maybe is already past the goal post. As a design system guy, I still have a few gripes with it.

### New syntax
Weakest arguments there is, it feels wrong to learn a new DSL (even if it's just text) that compiles back to the one you already know. Meh

### Tools required
To have a good time using tailwind, you need to get some extra tooling in place. 
- The class name ordering extension is a must, more on that very soon
- The watcher to build your style as you author them. Over the years the compiler has become better and better. But due to how tailwind works, it's difficult to ensure it works 100% of the time in edge case scenarios like interpolated class names etc.
- At some point, you will need something like CVA and/or tw-merge, more on that later.

### Nerfing your CSS.
Losing specificity and the cascade means working with a less capable tool. This is a trade-off to get a simpler more direct styling. Some will find this trade-off as beneficial.

#### Specificity
When using tailwind, the order you define conflicting classes in HTML have little to do about how your HTML nodes will be styled. After all, Tailwind and other atomic CSS libraries are *just CSS*, they need to abide by the same rules. The last declared class in the CSS file is the one that wins (and that file is written by someone else).
Two options are open to you as an atomic user: Use a tool in your editor to order the classes in the same order as the CSS (what tailwind does, and admittedly will help gzip compress better your HTML), and/or introduce runtime de-duplication (performance!?) like panda/styleX or CVA/tw-merge for tailwind.
All of this is handled by the speficity rules in regular CSS: Let's introduce now an un-named semantic CSS library: `.button` is the main style definition, `.button.importance-primary` can be a variant, `.button.importance-primary.flag-disabled` can be a compount variant. The styling engine in your browser handles all of that for you simply, automatically, and quickly. Atomic styling can't do that.

#### Styling at a distance
This is dangerous to go alone on this one, but crafting design systems with styling at a distance is great for intent communication:
- In design system you often need to target direct children, :after/before pseudo elements and so on
- Modifying the styles depending on the context of the element. If you make sure to always define it on the affected selector, life will be good
``` No bueno
.alert {
 background: red;
 color:white;
 & .button.delete: {
     text:red;
     background:white;
 }
}
```
``` bueno
.button.delete {
 background: red;
 color:white;
 .alert &: {
     text:red;
     background:white;
 }
}
```

### coloration of your styles / lack of coloration

### CSS moving target
Each new feature requires the Tailwind team (and other libraries like it) to implement it in their tools. This means in practice that the library is never "finished" and will always be in flux. Also means you have to wait for it to be in the library to use. Maybe once again, it's a good thing a protects you from yourselves. I like using @supports rules personnaly to give a little better experience to some of my users / try out features being flags.

## Current Production home
As mentionned before, I'm using stitches as my main styling library nowadays. It solves most of the issues mentionned above. But the company maintaining it got sold and is now been un-maintained for a few years. Still because it uses very standard CSS, I can work around most of its quirks by just declaring a little css here and there.

### Variant-first API is the way
As mentionned before, using specificity in CSS to handle variance is simple, clear and fast. barely any runtime costs.
It also makes reading the styles extremely clear, prevents extending styles for now reason. If styles are related but different, they might just be variants. (insert loki joke)
Discoverability of styles is greater, use your LSP to see what's available.
TypeScript has a field day inferring props.

### Theming done right
All the theme tokens are defined as CSS variables. Open for anyone to use/modify/override as you see fit. Dark theme? sure just define css variable under an @media query. Prefer using the color-scheme, sure go ahead.

### We're in the endgame now
If using an un-maintained library for more than 2 years without issue isn't a sign that we're finally getting to a stable place, I don't know what is.
A new feature drops into CSS, browser supports it, just gotta update your TS definitions and we can use it.



## Finally reaching the end goal post

The last remaining hurdles are 
- keeping up with React and other frontend libs/framework. Stitches breaks with RSC for instance.
- The exported type definitions are not ideal (and quite slow)

### Static extraction
Static extraction solves both problems by leveraging existing tools:
- CSS module injections: Already working with RSC, generative UIs
- Types just need to know about variants, the rest is generated at build time.

### Libraries

Some libraries to statically extract CSS-in-JS are 
 - vanilla extract (not ideal DX)
 - linaria
 - Pigment (very early)

But the tooling to build one yourself and do what you want is getting quite accessible
- Parcel Macros allows you to write css files and attach them to your bundler of choice. Inspired by bun macros and zig comp-time.
- CSS lightning lets you not worry about mimification, vendor pre-fixing and all of that.
- WyW-in-JS is a framework to build statically extracted libraries.
Using libraries like this removes a significant portion of the work and lets author focus on what's really important: The API (the fabled DX) and the trade-offs complexity/capabilities that fits their needs.

I hope to see an explosion of solutions, all targeting a slightly difference niche use-case. May the best/most generic/better weighted trade-off one wins


For instance Using Parcel Macros / Lightning CSS + dumb CSS generation means you are keeping up with CSS for close to 0 upkeep.


## Additional notes

### CSS winter is over
If you read Adam's blog post, the reasoning to go from semantic => atomic makes perfect sense. We are not living in this world anymore. A bunch of CSS capabilities showed up in the recent years and reshaped the landscape of CSS solutions.

@layer API is great, this is what you should ask your design system library to use. This prevents the coloration of your CSS tools you can use.

The cascade is a blessing, it wasn't before the :has selectors, interraction at a distance etc. Overwriting css Variables, easy peasy. Dialog and the Popover API fix one of the remaining issues with the cascade

 
### Generative UIs
At some point you are going to have to ships CSS along your components. Doing a big fat bundled css may works for a lot of use cases, but how do you deal with extra CSS, you will need/want critical CSS + extra anyway.


