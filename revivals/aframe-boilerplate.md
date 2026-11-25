<div align="center">

```
                         .   *        .       *
              *     .         ✦    .              .
                 ┌──────────────────────────────┐
           .     │  <a-scene>                   │    *
                 │    <a-box color="#4CC3D9">   │
         *       │    <a-sphere color="#EF2D5E">│  .
                 │    <a-sky color="#ECECEC">   │
              .  │  </a-scene>                  │       *
                 └──────────────┬───────────────┘
                                │  🌐 one html file
                                ▼
                       🥽  a whole world  🥽
                   (in your browser. in 2016!)
```

# 🌐 A-Frame Boilerplate

### *boilerplate for creating WebVR scenes using A-Frame.*

**forked: november 2016 · revived: november 2026 · the web learned to be 3D**

*a [GitHub Run](https://github.com/saitec/github-run) revival by [sai](https://5aitec.com)*

</div>

---

> 🦕 **honest intro.**
> this is a fork. i didn't write A-Frame. the brilliant folks at Mozilla's VR team did.
> in november 2016 i forked their starter kit to build VR scenes in the browser.
> the code is untouched. their original README is in [`README-2016.md`](README-2016.md).
> this page is my ten-year story with the 3D web.
> it was a wild ride. get comfy. 🛋️

---

## 📍 what's in this article

```
 1. 🍿  the short version
 2. 🌐  2016: "VR in the browser? lol"
 3. 🧩  why A-Frame was genius
 4. 🔧  what's inside this boilerplate
 5. 🍴  why i forked it
 6. 🌍  2016 → 2026: the world moved
 7. 🧭  WebVR → WebXR: the rename that mattered
 8. 🎨  the 3D web timeline
 9. 🤖  AI meets 3D
10. 🛠️  what i'd build now
11. 📚  lessons
12. 💌  p.s.
```

---

## 🍿 1. the short version

VR usually meant apps.
Big downloads. App stores. Game engines.
A-Frame said: what if VR was just a web page?
Write a few HTML tags.
Open it in a browser.
Put on a headset. You're inside it. 🥽

This repo is the starter kit for that.
One `index.html`. A box, a sphere, a cylinder, a sky.
I forked it in 2016 to build my own scenes.
Ten years later, it's a time capsule
of the moment the web tried to go 3D.

---

## 🌐 2. 2016: "VR in the browser? lol"

Okay, rewind to late 2016.

VR was the hottest thing in tech.
The Rift and Vive had shipped that year.
PlayStation VR landed in October.
Google Daydream came out in November.
Cardboard viewers were everywhere. 📦
You put your phone in a cardboard box and looked around.
Janky. Magical. Very 2016.

Most VR was built in game engines.
Unity. Unreal.
Heavy tools. Steep learning curves.
To share a VR thing, you shipped an app.
App store review. Downloads. Updates. Friction.

And then a bunch of people at Mozilla asked a big question.
**What if VR worked like the web?**

Click a link. You're in.
No install. No store.
Share a URL with a friend. They're in too. 🔗

That's the WebVR dream.
Browsers would talk to headsets directly.
A web page could render 3D at headset frame rates.
Anyone with HTML skills could build worlds.

People laughed at first.
"The browser? For VR? It can barely scroll." 😂
But the demos started working.
And they worked surprisingly well.

```
   HOW YOU SHARED VR, 2016 📤

   🎮 game engine way:                 🌐 web way:
   ─────────────────────               ─────────────────────
   1. build in Unity                   1. write some HTML
   2. export per platform              2. put it online
   3. submit to a store                3. send a link 🔗
   4. wait for review                  4. friend clicks it
   5. friend downloads 2 GB            5. friend is in VR 🥽
   6. friend gives up 😩
```

---

## 🧩 3. why A-Frame was genius

A-Frame did one brilliant thing.
It made 3D feel like HTML.

Look at this:

```html
<a-scene>
  <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
  <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
  <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
  <a-plane position="0 0 -4" rotation="-90 0 0" width="4" height="4" color="#7BC8A4"></a-plane>
  <a-sky color="#ECECEC"></a-sky>
</a-scene>
```

That's a whole VR scene.
A blue box. A pink ball. A yellow tube. A green floor. A grey sky.
Five lines.
If you can write a `<div>`, you can build a world. 🌍

My fork loads A-Frame version 0.3.2 straight from aframe.io.
Zero point three. 🍼
A baby framework, already doing VR in a browser.

Under the hood, A-Frame sat on three.js.
three.js is the giant of 3D on the web.
It talks to WebGL, the browser's graphics engine.
A-Frame wrapped all that in friendly tags.

```
   THE A-FRAME LAYER CAKE 🎂

   ┌──────────────────────────────────┐
   │ 🧑‍🎨 you: <a-box color="blue">    │  ← friendly HTML
   ├──────────────────────────────────┤
   │ 🧩 A-Frame: entities + components │  ← the clever part
   ├──────────────────────────────────┤
   │ 🔺 three.js: scenes, meshes, cams │  ← the 3D engine
   ├──────────────────────────────────┤
   │ 🖥️ WebGL: talk to the GPU         │  ← browser graphics
   ├──────────────────────────────────┤
   │ 🥽 WebVR: talk to the headset     │  ← (became WebXR later)
   └──────────────────────────────────┘
```

### 🧩 the entity-component idea

The other genius bit was how it organised things.
It's called entity-component-system.
Big words. Simple idea.

- An **entity** is a thing. A box. A light. A player.
- A **component** is a trait. Colour. Position. "Spins". "Can be grabbed".
- You build things by mixing traits.

Want a box that spins and glows?
Add a "spin" component and a "glow" component.
Want a sphere that spins too?
Add the same "spin" component.
No copy-paste. Just mix and match. 🧪

Game engines used this pattern for years.
A-Frame brought it to web developers.
And the community went wild building components.
Physics. Teleporting. Hand controls. Particles. Text.
A whole ecosystem of Lego bricks. 🧱

---

## 🔧 4. what's inside this boilerplate

The repo is tiny. Beautifully tiny.

```
aframe-boilerplate/
├── 🌐 index.html      ← the whole scene. the whole app.
├── 📦 package.json    ← a tiny dev server: npm start
├── 📜 LICENSE         ← MIT, from Mozilla
└── 📖 README.md       ← three ways to get started
```

The original README offered three ways in.

**🗜️ Option 1: download the zip.**
Unzip. `npm install && npm start`.
Open `localhost:3000`. Done.

**🍴 Option 2: fork the repo.**
That's what I did.
Clone your fork. Same two commands.

**✒️ Option 3: fork a CodePen.**
Edit it live in the browser.
No install at all.
Very 2016. Very lovely.

And it had this tip I adored:

> 📱 **Mobile pro tip:** load the dev server URL on your phone.
> If they're not on the same network, try a tunnelling tool.

Phone in a cardboard viewer.
Laptop running the server.
Edit code. Refresh phone. Look around. 🔄
That loop was addictive.
Building worlds at your desk, testing them on your face.

The README even had emoji in the headings.
📦, 🍴, 🐙, 💾, ✒️.
In 2016, that felt rebellious for docs.
Now every README does it.
(Including this one. Hi. 👋)

---

## 🍴 5. why i forked it

In 2016, I was deep in AR and VR.
I wanted to prototype ideas fast.
Unity felt heavy for quick experiments.
A-Frame let me sketch a scene in minutes.
Show a client a link. They put on a headset. Boom.

That speed mattered.
It changed how I thought about 3D.
Not as big productions.
As quick, shareable sketches. ✏️

The commit history of this fork is tiny.
The last commit says "remove old yml file".
That's it. That's my contribution. 😂
Housekeeping on someone else's masterpiece.

But forks aren't always about code.
Sometimes a fork is a bookmark.
"This matters. I'm coming back."
Ten years later, I came back.
And honestly, that's how a lot of my work goes.
I save something. I forget it. Life moves.
Then one day it clicks into a new project.
The 3D web clicked back in for me
when glasses and AI-made 3D showed up.
Suddenly this little fork felt relevant again.

---

## 📦 bonus: a cardboard afternoon, 2016

Let me take you back to a real-feeling afternoon.
You'll recognise it if you were there.

It's 3pm.
A small office. Fans on full. ☀️
On the desk: a laptop, a phone, and a cardboard box.
Not a shipping box. A VR viewer.
Folded cardboard, two plastic lenses, a rubber band.

You write five lines of A-Frame.
A box. A sky. A floor.
`npm start`.
You type the laptop's address into your phone's browser.
The scene loads. Two little circles, side by side.
You tap the VR button. 📱
Slide the phone into the cardboard.
Hold it up to your face.

And you're standing in a grey room.
A blue box floats in front of you.
You turn your head. The world turns. 🌍
It's low-res. A bit laggy. Slightly blurry.
And it's the coolest thing you've ever seen.

You call someone over.
"Look at this."
They hold the cardboard up.
"Whoa." 😮
Then they reach out to touch the box.
Their hand hits air.
Everyone laughs.

Then you go back and add a spinning sphere.
Refresh. Phone back in the box.
Repeat until 9pm. 🌙

```
   THE 2016 DEV LOOP 🔁

   ✏️ edit index.html
      │
      ▼
   🔄 refresh phone
      │
      ▼
   📦 phone into cardboard
      │
      ▼
   😮 "whoa"
      │
      ▼
   🤚 try to touch the box. fail. laugh.
      │
      └──────► back to ✏️
```

That loop taught a generation of builders 3D.
No game engine. No license. No big PC.
A browser, a phone, and a cardboard box.

In 2026 the loop is smoother.
A headset that sees your hands.
Hand tracking means you *can* touch the box.
Sort of. 🤏
And an AI writes half the scene before you start.
But the "whoa" moment?
Still the same.
That never gets old.

---

## 🛍️ bonus: 3D shopping, then and now

My job now is commerce.
So I can't skip this bit.

### 🦕 2016

Brands loved the idea of 3D shopping.
Virtual stores! Walk the aisles in VR!
A few big brands built showcase experiences.
Mostly for press. Rarely for sales.
The problems were obvious:
- Nobody owned a headset. 🥽❌
- 3D models of products cost a lot to make.
- Walking down a virtual aisle is slower than scrolling.

The fun stat-free truth?
Most people just wanted to see the shoe from the side.
Not walk through a virtual mall to find it. 👟

### 🚀 2026

3D shopping got real, but in a smaller shape.
- 📱 **AR on phones.** Place the sofa in your room before buying. Normal now.
- 🔄 **360 product spins.** On almost every good product page.
- 👓 **Try-ons.** Glasses, makeup, shoes, through your camera.
- 🤖 **AI-made 3D.** One photo in, a 3D product out. Cost fell off a cliff.
- ✨ **Splat stores.** Scan a real shop. Let people peek in online.

```
   WHAT 3D SHOPPING BECAME 🛍️

   2016 dream:  🏬 walk a virtual mall in VR
                      │
                      ▼  (reality check)
   2026 reality: 👟 spin the shoe · 🛋️ place the sofa · 👓 try the frames
```

The dream shrank. The value grew.
Small, useful 3D beats big, flashy 3D.
That's a lesson I carry into ShopOS.
Don't build the virtual mall.
Build the button that answers "will it fit my room?"

---

## 🌍 6. 2016 → 2026: the world moved

Big picture.
How different is the world since this fork?

```
   2016                                          2026
    │                                              │
 📦 cardboard VR viewers      ──────────►  🥽 standalone headsets
 🌐 WebVR, experimental       ──────────►  🌐 WebXR, a real web standard
 🖥️ WebGL only                ──────────►  ⚡ WebGPU in modern browsers
 🧊 3D = polygons             ──────────►  ✨ 3D = polygons + gaussian splats
 🧑‍🎨 humans model every asset ─────────►  🤖 AI generates 3D from a prompt
 🍎 Apple: no headset         ──────────►  🍎 Vision Pro, with a WebXR browser
 😷 "pandemic" = a movie      ──────────►  😷 we lived it. virtual events spiked.
 🐙 GitHub: indie             ──────────►  🐙 GitHub: Microsoft + Copilot
```

### 📦 cardboard died, headsets grew up

Phone-in-a-box VR faded fast.
Google stopped pushing Cardboard and Daydream within a few years.
Standalone headsets won.
No phone. No PC. Just a headset with a chip inside.
Quest made that normal.
Then mixed reality arrived.
See your real room. Put virtual stuff in it.

### 😷 the pandemic made virtual spaces matter

In 2020, nobody could meet in person.
Virtual events, galleries, campuses popped up.
Many ran in the browser.
Some used A-Frame or three.js.
Click a link, walk around a virtual expo.
It wasn't perfect. Avatars were awkward. 🙃
But the web was the easiest door in.
No install was the killer feature.

### 🦊 Mozilla stepped back from VR

This one stings a bit.
Mozilla created A-Frame and pushed WebVR hard.
In 2020, Mozilla cut a lot of its emerging tech work.
Its VR browser was handed to another open-source company,
who kept it alive under a new name.
A-Frame itself lived on through its community and maintainers.
That's the beauty of open source.
The parent can leave. The kid keeps growing. 🌱

### 🍎 Apple joined, and brought the web along

Apple's Vision Pro shipped in early 2024.
Its Safari browser added WebXR support.
That's huge for the web dream.
A link that opens in VR on Apple's headset too.
No App Store needed.
2016 me would have screamed. 📣

---

## 🧭 7. WebVR → WebXR: the rename that mattered

In 2016 the standard was WebVR.
It only cared about virtual reality.
Fully immersive. Block out the room.

Then AR got big.
Phones could place 3D stuff on your table.
Headsets started showing your real room.
So "VR" alone felt too narrow.

The standard was rebuilt as WebXR.
X stands for "anything".
VR, AR, mixed, whatever comes next.
Chrome shipped WebXR around the end of 2019.
A-Frame moved over to it with its 1.0 release, around the same time.
Other browsers followed over the years.

```
   THE STANDARD'S GLOW-UP 🦋

   2016  🥽 WebVR         "VR only. experimental flags. good luck."
           │
           ▼
   2019  🌐 WebXR         "VR + AR. one API. ships in Chrome."
           │
           ▼
   2020s 📱 AR on phones   "drop a 3D sofa in your living room"
           │
           ▼
   2024  🍎 Vision Pro     "Safari speaks WebXR too"
           │
           ▼
   2026  👓 glasses        "the web, overlaid on the world"
```

My fork is frozen in the WebVR era.
The code still says "WebVR" in the README.
Like a phone that says "3G" on the box. 📦
Cute. Dated. Honest.

---

## 🎨 8. the 3D web timeline

Let me stack the whole decade.

```
 2011 ┤ 🖥️ WebGL arrives            browsers can draw 3D with the GPU
 2010s┤ 🔺 three.js becomes king      the go-to 3D library for the web
      │
 2015 ┤ 🧩 A-Frame launches           3D as HTML tags (Mozilla VR team)
      │
 2016 ┤ 🦕 THIS FORK                  cardboard, WebVR flags, big dreams
      │ 📦 cardboard + daydream      phone VR peaks
      │
 2019 ┤ 🌐 WebXR ships in Chrome      VR + AR, one standard
      │ 🧩 A-Frame 1.0                built for WebXR
      │
 2020 ┤ 😷 virtual events boom        browser 3D spaces everywhere
      │ 🦊 Mozilla steps back         community carries A-Frame on
      │
 2023 ┤ ⚡ WebGPU in Chrome            modern GPU access, way faster
      │ ✨ gaussian splatting          photoreal scenes from photos
      │
 2024 ┤ 🍎 Vision Pro + WebXR          Apple's headset joins the web
      │ 🤖 text-to-3D models           describe it, get a mesh
      │
 2025 ┤ ⚡ WebGPU in more browsers     the web gets serious graphics
      │
 2026 ┤ 👓 AI glasses                  the next screen is your whole view
      ▼
```

Two of these deserve a closer look.

### ⚡ WebGPU

WebGL was based on graphics tech from the 2000s.
It worked. But it was old.
WebGPU is the modern version.
Closer to how today's GPUs actually work.
Faster. More control. Compute shaders too.
That last bit means you can even run AI models in the browser on the GPU. 🤯

### ✨ gaussian splatting

This one is magic. Real magic. 🪄
In 2023 a new technique took off.
Take a bunch of photos of a place.
A model turns them into millions of fuzzy coloured blobs.
Render the blobs from any angle.
Result: a photoreal 3D scene you can walk through.

No modelling. No polygons. Just photos.
And it runs in a browser.
Walk through a real shop. A real street. A real room.
From a link. 🔗

For 2016 A-Frame scenes, everything was boxes and spheres.
In 2026, you can drop a photoreal splat of a real place
into a web page.
That's a giant leap.

---

## 🔺 bonus: a love note to three.js

I can't talk about A-Frame without three.js.
It's the quiet engine under so much of the 3D web.
Product viewers. Portfolio sites. Data globes. Games.
Award-winning websites with floating, spinning, glowing things.
A lot of it is three.js underneath.

It started as one person's side project.
Then thousands of contributors piled in.
It's been going for well over a decade.
New versions keep landing.
It even grew a WebGPU renderer as browsers caught up.

```
   THE 3D WEB FAMILY TREE 🌳

             🖥️ WebGL / ⚡ WebGPU
                     │
                 🔺 three.js
         ┌───────────┼────────────┐
         ▼           ▼            ▼
   🧩 A-Frame   ⚛️ react-three   🎨 a million
   (HTML tags)   (React folks)    creative sites
```

If A-Frame is the friendly front door,
three.js is the house. 🏠
Learn A-Frame to fall in love.
Learn three.js to build anything.
Both are free. Both are open.
Both are still thriving in 2026.
That's rare for web tech from 2016. 🫶

---

## 🤖 9. AI meets 3D

Of course AI showed up in 3D too.

In 2016, every 3D object was made by hand.
An artist in Blender. Hours or days per object.
For a small team, that was the bottleneck.
You wanted a café scene?
Someone had to model every cup. ☕

In 2026:
- 🗣️ **Text to 3D.** Type "a cosy café table with two cups". Get a mesh.
- 📸 **Photo to 3D.** One photo of a sneaker. Get a 3D sneaker.
- 🎬 **Video to scene.** Walk around with your phone. Get a splat.
- 🧑‍💻 **Code to scene.** Ask an AI to write the A-Frame or three.js code. It does. Pretty well.

```
   MAKING A 3D SCENE 🛠️

   2016:  🧑‍🎨 model in Blender (days)
          📤 export
          🧩 write A-Frame tags by hand
          🔄 tweak positions for hours

   2026:  🗣️ "a product showroom with three pedestals and soft light"
          🤖 AI writes the scene + generates the objects
          🧑‍🎨 you tweak taste, not coordinates
          🔗 share a link
```

At ShopOS this matters a lot.
Brands want their products in 3D.
Spin a shoe. Peek inside a bag. Place a lamp in your room.
In 2016 that cost a small fortune per product.
Now AI can draft the 3D asset.
Humans polish. Ship to the web.
Same "share a link" dream as A-Frame.
Just way cheaper to fill with stuff.

And the AI also writes the A-Frame code now.
"Build me a VR gallery for these ten images."
Thirty seconds later, there's a working scene.
That's wild.
The boilerplate I forked is now something an AI writes for you. 🤖✍️

---

## 🛠️ 10. what i'd build now

Okay. If I rebuilt this repo today.
Not a fork. My own thing.

### 🎯 the idea: "link to a world" for brands

A starter kit for 3D product spaces on the web.
Open in a browser. Works on phones, headsets, and glasses.
Filled with AI-made assets.
Shareable with one link.

### 🏗️ the shape

```
   LINK-TO-A-WORLD KIT 🌐🛍️

   📸 product photos / 🗣️ a text brief
          │
          ▼
   ┌────────────────────┐
   │ 🤖 3D asset maker   │  ← photo → mesh, or text → mesh
   └─────────┬──────────┘
             ▼
   ┌────────────────────┐
   │ ✨ splat backdrop   │  ← a real store, scanned with a phone
   └─────────┬──────────┘
             ▼
   ┌────────────────────┐
   │ 🧩 scene builder    │  ← A-Frame / three.js, written by an agent
   └─────────┬──────────┘
             ▼
   ┌────────────────────┐
   │ ⚡ WebGPU renderer  │  ← fast on new devices, WebGL fallback
   └─────────┬──────────┘
             ▼
   ┌────────────────────┐
   │ 🥽 WebXR mode       │  ← tap "enter VR/AR" on any headset
   └─────────┬──────────┘
             ▼
        🔗 one link. send it anywhere.
```

### 🧰 what's inside

- **A-Frame, still.** Friendly tags never go out of style.
- **A splat component.** Drop in a scanned room.
- **AI asset slots.** Point to a product photo. Get a 3D model.
- **A Claude skill.** "Make a 3D showroom for these products."
- **Analytics that respect privacy.** Which product did people look at longest?
- **Glasses mode.** Small overlays. Not a whole world. Just the product, floating.

### 🔗 where it fits my life

At ShopOS, our Sloosh platform already makes images and videos for brands.
3D is the obvious next format.
At SeeIt, we build glasses.
Glasses need the web to work in 3D.
This boilerplate's spirit sits right between the two.
Not a fossil. A seed. 🌱

---

## 📚 11. lessons

What did this tiny fork teach me over ten years?

**1. Lower the floor, and the world walks in.** 🚪
A-Frame's magic was "if you know HTML, you can do VR".
Every big shift does this.
Make the hard thing feel familiar.

**2. Links beat installs.** 🔗
No-install was the killer feature in 2016.
And in 2020. And in 2026.
Friction kills more ideas than bad ideas do.

**3. Standards get renamed. Ideas don't.** 🦋
WebVR became WebXR.
The idea, "3D on the open web", never changed.
Bet on the idea. Expect the name to shift.

**4. Communities outlive companies.** 🫂
Mozilla stepped back.
A-Frame kept going.
Open source with a real community is hard to kill.

**5. Forks can be bookmarks.** 🔖
I barely touched this fork.
But it reminded me, for ten years, that I cared.
Some repos are code. Some are intentions.

**6. Boxes first, then reality.** 📦➡️🌍
2016 scenes were boxes and spheres.
2026 scenes are photoreal splats.
Start simple. Reality catches up.

**7. AI eats the boilerplate.** 🤖
Starter kits used to be gold.
Now an agent writes your starter kit in seconds.
What stays valuable? Knowing what to build. And taste.

**8. The "whoa" is the product.** 😮
Nobody remembers the frame rate.
They remember the moment the room appeared around them.
Design for that first ten seconds.
Everything else is plumbing.
Good plumbing matters. But the whoa sells it.

**9. Small and useful beats big and flashy.** 🛋️
The virtual mall flopped.
The "see it in your room" button won.
Ship the smallest thing that answers a real question.

---

## 🗺️ the cheat sheet

| | 2016 🦕 | 2026 🚀 |
|---|---|---|
| 🥽 how you do VR | phone in cardboard, or a big PC | standalone headset |
| 🌐 the web standard | WebVR (experimental) | WebXR (VR + AR) |
| 🖥️ browser graphics | WebGL | WebGPU, WebGL fallback |
| 🧊 3D content | hand-made polygons | polygons + splats + AI-made |
| 🍎 Apple in XR | nowhere | Vision Pro, WebXR in Safari |
| 🦊 Mozilla in VR | leading the charge | stepped back; community carries on |
| 🧑‍💻 writing the scene | you, by hand | you + an AI agent |
| 🔗 sharing it | a link | still a link 🫶 |

---

## 🏃 running it (it might just work!)

This one has a decent chance of working.
HTML ages better than most code. 💪

```bash
git clone https://github.com/saitec/aframe-boilerplate
cd aframe-boilerplate
npm install && npm start
open http://localhost:3000/
```

If the old A-Frame version struggles in a modern browser,
swap the script tag to a current A-Frame release.
Same tags. Same scene. Now with WebXR. 🌐
That's the joy of a standards-based web.
Old pages keep working, mostly. 🙌

---

## 🧑‍🏫 bonus: teach a kid 3D in ten minutes

This is my favourite use of A-Frame. Still.
If you have a kid around, or a curious friend, try this.

**Minute 1.** Open any online code editor. 🖥️
**Minute 2.** Paste the five-line scene from earlier.
**Minute 3.** Change `color="#4CC3D9"` to `color="red"`. Watch it change. 🔴
**Minute 4.** Change the box's `position`. Watch it move. ↔️
**Minute 5.** Add another `<a-sphere>`. Now there are two balls. ⚽⚽
**Minute 6.** Add `animation="property: rotation; to: 0 360 0; loop: true; dur: 4000"`. It spins! 🌀
**Minute 7.** Change the sky colour to `#000000`. Night time. 🌙
**Minute 8.** Add a bunch of tiny white spheres. Stars. ✨
**Minute 9.** Open it on a phone. Look around.
**Minute 10.** Watch their face. 😮

That's it.
They just built a world.
With tags they can read.
No install. No account. No app store.

That's what A-Frame got so right.
Not the tech. The *feeling*.
"I made this. And I'm standing inside it."
Every good creative tool gives you that feeling fast.

In 2026 you can do the same with an AI.
"Make me a night sky with spinning planets."
It writes the A-Frame for you.
Faster. Fancier.
But I'd still start a kid with the tags.
Typing `color="red"` and seeing red teaches something an AI can't.
It teaches you that you're in control. 🎮

## 🙋 who should care in 2026

- 🛍️ **Brands.** Small 3D (spins, try-ons, room placement) sells. Big 3D (virtual malls) rarely does.
- 👓 **Glasses builders.** The web will be how most people get 3D on their face.
- 🧑‍🎓 **Students and kids.** A-Frame is still the friendliest door into 3D.
- 🤖 **AI tool builders.** Text-to-scene is a great product. Make it output A-Frame or three.js.
- 🦊 **Old Mozilla VR fans.** Your idea won. It just took a decade. 🫶

## 💌 12. p.s.

To 2016 me:
You forked a starter kit and committed one cleanup.
That's okay. You were sketching.
The web did become 3D.
Slower than you thought. Bigger than you dreamed.

To the A-Frame folks, past and present:
Thank you. 🙏
For the tags. For the components. For the docs with emoji.
For keeping it alive when the big company stepped away.
You made VR feel like the web.
That idea still guides how I think about 3D.

To you, reading:
If you're building 3D on the web, splats, WebXR, or AI-made worlds,
come say hi at [@5aitec](https://x.com/5aitec) or [5aitec.com](https://5aitec.com).
Send me a link. I'll put on a headset. 🥽

---

<div align="center">

```
   2016:  <a-box color="#4CC3D9">   🥽 in cardboard
   2026:  "make me a showroom"      🤖 ✨ 👓 on any device
   ──────────────────────────────────────────────────
          the web learned to be 3D. one link at a time.
```

*part of the [GitHub Run](https://github.com/saitec/github-run): one fix, one insight, every day.*

</div>
