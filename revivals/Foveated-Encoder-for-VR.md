<div align="center">

```
    ███████╗ ██████╗ ██╗   ██╗███████╗ █████╗ ████████╗███████╗
    ██╔════╝██╔═══██╗██║   ██║██╔════╝██╔══██╗╚══██╔══╝██╔════╝
    █████╗  ██║   ██║██║   ██║█████╗  ███████║   ██║   █████╗
    ██╔══╝  ██║   ██║╚██╗ ██╔╝██╔══╝  ██╔══██║   ██║   ██╔══╝
    ██║     ╚██████╔╝ ╚████╔╝ ███████╗██║  ██║   ██║   ███████╗
    ╚═╝      ╚═════╝   ╚═══╝  ╚══════╝╚═╝  ╚═╝   ╚═╝   ╚══════╝
                    👁️  see sharp where it matters  👁️
```

# 👁️ Foveated Encoder for VR

**pushed: september 2016 · revived: november 2026 · ten years, one eyeball**

*a [GitHub Run](https://github.com/saitec/github-run) revival by [sai](https://5aitec.com)*

</div>

---

> 🦕 **heads up.** this README is new. the code is not.
> the code is exactly what 2016 me pushed. C++, CUDA, OpenCV, an Oculus Rift.
> the original README lives in [`README-2016.md`](README-2016.md).
> this page is the story. what it was. what changed. what i'd build now.
> grab a chai. it's a long one. ☕

---

## 📍 table of contents

```
 1. 🍿  the one-line pitch
 2. 🕰️  september 2016, a vibe check
 3. 🧠  what foveation even is
 4. 🔧  what's actually inside this repo
 5. 🌍  ten years later: how much the world moved
 6. 🥽  the headset timeline, 2016 → 2026
 7. 🧮  the compute timeline
 8. 🤖  the AI plot twist nobody saw coming
 9. 🛠️  what i'd build now
10. 📚  lessons from a ten-year-old repo
11. 💌  p.s.
```

---

## 🍿 1. the one-line pitch

Your eye only sees sharp in a tiny spot.
Everything else is blur your brain fills in.
So why render the whole frame in 4K?
Render sharp where you look. Blur the rest.
Save a ton of pixels. Nobody notices.

That's foveated rendering.
This repo tried it in 2016.
On a first-gen Oculus Rift.
With a lot of optimism and not much eye tracking.

---

## 🕰️ 2. september 2016, a vibe check

Okay. Let's set the scene.

It's September 2016.
I'm deep in AR and VR land.
Everyone I know is strapping screens to their face.
The Oculus Rift consumer version shipped that spring.
The HTC Vive came right after.
Sony was about to drop PlayStation VR.
Pokémon Go had just broken the internet over summer.
Every pitch deck had the word "immersive" in it.

The mood was simple.
VR is the next platform.
It's coming next year. For sure.
(Spoiler: it took a little longer.)

```
         2016 MOOD BOARD 📌
  ┌─────────────────────────────────┐
  │ 🥽 Rift ships!   🎮 Vive ships! │
  │ 👾 Pokémon Go eats the world    │
  │ 🧠 AlphaGo beats a Go champion  │
  │ 📱 phones get two cameras       │
  │ 💬 "chatbots are the new apps"  │
  │ 🚀 "VR is next year, trust me"  │
  └─────────────────────────────────┘
```

And there was one huge problem.
VR was heavy. Like, really heavy.
Two eyes. Two screens.
Ninety frames every second. No dropped frames.
Drop one and people feel sick. 🤢
The GPUs that could do this cost a fortune.
The cables were thick as a garden hose.
You needed a gaming PC humming under your desk.

Now add 360 video on top.
Video in VR is a giant sphere of pixels.
You only look at a slice of it.
But you still stream and decode all of it.
That's a lot of wasted bits.

So the question in my head was simple.
**What if we only spend pixels where the eye looks?**

That question became this repo.

---

## 🧠 3. what foveation even is

Let's do a tiny biology class. Promise it's fun.

Your retina has a little pit in the middle.
It's called the fovea.
It's packed with cone cells.
That's where your sharp, colourful vision lives.
It covers a tiny slice of what you see.
Hold your thumb out at arm's length.
Your thumbnail is roughly the sharp zone.
Everything outside that gets blurrier and blurrier.

You don't notice because your eyes jump around.
Those jumps are called saccades.
Your brain stitches the sharp bits together.
It feels like one crisp world.
It's actually a clever lie. 🧙

```
        what you THINK you see        what your eye ACTUALLY gets
     ┌─────────────────────────┐    ┌─────────────────────────┐
     │█████████████████████████│    │░░░░░░░░░░░░░░░░░░░░░░░░░│
     │█████████████████████████│    │░░░░░▒▒▒▒▒▒▒▒▒▒▒▒▒░░░░░░░│
     │█████████████████████████│    │░░░▒▒▒▒▓▓▓▓▓▓▓▓▒▒▒▒░░░░░░│
     │████████████ 👁️ █████████│    │░░▒▒▒▓▓▓▓ 👁️ ▓▓▓▓▒▒▒░░░░░│
     │█████████████████████████│    │░░░▒▒▒▒▓▓▓▓▓▓▓▓▒▒▒▒░░░░░░│
     │█████████████████████████│    │░░░░░▒▒▒▒▒▒▒▒▒▒▒▒▒░░░░░░░│
     │█████████████████████████│    │░░░░░░░░░░░░░░░░░░░░░░░░░│
     └─────────────────────────┘    └─────────────────────────┘
          sharp everywhere              sharp in the middle only
```

So the trick for computers is obvious.
Copy the eye.
Full detail in the middle ring.
Medium detail in the next ring.
Low detail at the edges.

There are two flavours of this.

**Fixed foveation.** 📌
Assume the user looks at the centre.
Sharp middle, soft edges. Always.
Cheap. Works okay. Breaks when you glance sideways.

**Dynamic foveation.** 👀
Track the eye with a camera inside the headset.
Move the sharp zone wherever the eye goes.
Way better. Way harder.
You need fast, accurate eye tracking.
In 2016, consumer headsets didn't have it.

So this repo lived in the fixed world.
With dreams of the dynamic one.

And people forget this bit.
Foveation isn't only for rendering games.
It works for video too.
Encode the gaze region at high quality.
Encode the rest at low quality.
Ship way fewer bits.
That's the "encoder" in Foveated Encoder.

---

## 🔧 4. what's actually inside this repo

Let's open the box. 📦
I went through the code fresh, ten years later.
It's like reading your old diary.
Cute. A bit embarrassing. Mostly proud.

```
Foveated-Encoder-for-VR/
├── 🧩 main.cpp          ← one binary, many "runmodes"
├── 🛠️ CMakeLists.txt    ← the build
├── 🔍 FindOculus.cmake  ← hunting for the Oculus SDK
├── ⚙️ settings.hpp
├── 📐 contracts.h
├── ⏱️ CycleTimer.h      ← how fast is fast?
├── 🧪 buildtest/        ← a CUDA kernel (.cu!) to check the GPU works
├── 🥽 oculus2/          ← talking to the Rift
├── 🎨 renderer/         ← the drawing part
├── 🧪 rendertest/       ← does the drawing work?
├── 🎯 optimizer/        ← the "spend pixels wisely" part
├── 🎞️ videoreader/      ← OpenCV reading video frames
└── 🧰 util/
     ├── cylinderwarp    ← bending flat video onto a curve
     ├── imageutil
     ├── timer
     └── workqueue       ← threads doing chores in parallel
```

The heart is `main.cpp`.
It's one program with a bunch of modes.
You pick a mode when you run it.

```
$ ./foveate
Please provide a runmode.
Available runmodes:
  buildtest      🧪  is CUDA alive?
  playvideo      🎞️  just play a file
  cylinderwarp   🌀  bend video onto a cylinder
  render         🎨  draw the scene
  rendertest     ✅  test the drawing
  oculus2        🥽  push it to the Rift
  optimize       🎯  find the cheapest good-looking frame
```

I love this pattern still.
One binary. Many little tools inside.
Every mode is a tiny experiment.
You can poke one piece without breaking another.

**`playvideo`** is the hello world.
Open a file with OpenCV.
Show frame after frame.
If this breaks, nothing else matters.

**`cylinderwarp`** is the fun one. 🌀
Flat video doesn't wrap around your head.
So you bend it onto a cylinder.
Every pixel gets a new spot.
It's a bit of trig and a lot of patience.

**`buildtest`** is a CUDA sanity check.
CUDA is how you talk to Nvidia GPUs directly.
In 2016 that meant fighting drivers for days. 😤
This mode just asks the GPU: are you awake?

**`oculus2`** talks to the headset.
Oculus SDK, version two-ish era.
Hand it frames. Pray for 90 fps.

**`optimize`** is the brain. 🎯
This is where the foveation lives.
Split the frame into regions.
Give the centre the most detail.
Squeeze the edges hard.
Measure. Tweak. Measure again.

```
   ONE FRAME, THREE BUDGETS 🎯

   ┌───────────────────────────────────┐
   │  low  low  low  low  low  low  low│   💸 cheap
   │  low ┌───────────────────────┐ low│
   │  low │ mid   mid   mid   mid │ low│   💰 medium
   │  low │ mid ┌───────────┐ mid │ low│
   │  low │ mid │  👁️ FULL   │ mid │ low│   💎 expensive
   │  low │ mid └───────────┘ mid │ low│
   │  low │ mid   mid   mid   mid │ low│
   │  low └───────────────────────┘ low│
   │  low  low  low  low  low  low  low│
   └───────────────────────────────────┘
```

**`workqueue.h`** is a small thread pool.
Chop the work into chunks.
Let many CPU threads chew at once.
Classic 2016 move before everything moved to GPUs.

Is it production code? Nope. 🙃
It's a research toy.
It's a "can this even work" repo.
But that's the point of a repo like this.
You don't ship it. You learn from it.

What it got right:
- The idea. Foveation was the future. It is now the present.
- The split into small runmodes. Very testable.
- Video plus rendering together. Ahead of its time a bit.

What it got wrong:
- No eye tracking. Fixed centre only.
- Tied hard to one headset SDK.
- Zero tests beyond "does it run on my machine".
- The README promised more than the code did. Classic founder energy. 😅

---

## 🌍 5. ten years later: how much the world moved

Okay this is the fun part.
I pushed this repo in September 2016.
It's November 2026 now.
Let's zoom out.
How different is the world?

Honestly? Wildly different.
Let me paint it.

```
   2016                                          2026
    │                                              │
    ▼                                              ▼
 📱 phones with two cameras  ──────────►  📱 phones that run AI models
 🥽 VR tethered to a big PC  ──────────►  🥽 headsets with chips inside
 👓 smart glasses = a joke   ──────────►  👓 glasses with cameras & AI
 💬 chatbots follow scripts  ──────────►  🤖 agents that write code
 🎮 GTX 1080 is a beast      ──────────►  🧮 data centres full of GPUs
 🎥 360 video is the future  ──────────►  🎬 AI generates video from text
 🐙 GitHub, scrappy & indie  ──────────►  🐙 GitHub, owned by Microsoft
 😷 "pandemic" = a movie      ──────────►  😷 we all lived through one
```

Let me walk through it slowly.

### 😷 we went through a whole pandemic

This sounds dramatic. It was.
In 2020 the world just stopped.
Offices closed. Everyone went remote.
Video calls became the office.
Suddenly "meet in VR" wasn't a sci-fi line.
People actually tried it.
Most went back to Zoom. 😂
But headsets sold like crazy for a while.
Being stuck at home makes a portal look good.

### 🏢 Facebook became Meta

In 2016 Facebook owned Oculus.
By late 2021 the whole company renamed itself Meta.
Bet the company on the "metaverse".
Spent huge amounts on it, year after year.
The metaverse part got memed hard.
But the hardware part? That paid off slowly.
The Quest line made VR affordable.
And the glasses work became real.

### 🍎 Apple finally showed up

Apple stayed quiet for years.
Then in early 2024 the Vision Pro shipped in the US.
Expensive. Heavy. Very, very polished.
And guess what it leans on?
**Eye-tracked foveated rendering.** 👁️
The exact thing this repo dreamed about.
Your eyes are the mouse.
Look at a button. Pinch your fingers. Done.
The sharp zone follows your gaze.
Everywhere else gets cheaper.

Reading the Vision Pro tech notes felt personal.
Like seeing your kid's drawing in a museum.
Not my drawing. But the same idea.

### 👓 glasses got real

In 2016 smart glasses were a punchline.
Google Glass had already flopped.
Snap Spectacles were a toy with a camera.

Now? Ray-Ban Meta glasses sell well.
Camera. Speakers. An AI you talk to.
Meta showed a true AR glasses prototype in 2024.
Displays in the lens started showing up in 2025.
Google came back with Android XR.
Samsung shipped a headset on it.

And honestly, this is personal.
I co-founded SeeIt, an open-source AI glasses lab.
So foveation isn't nostalgia for me.
It's homework. 📓
Glasses have tiny batteries.
Tiny chips. Tiny heat budgets.
Every pixel you skip is battery you keep.
Foveation matters more on glasses than headsets.

### 🤖 AI ate everything

This is the big one.
In 2016, "AI" meant image classifiers.
Is this a cat? Is this a hot dog? 🌭
AlphaGo beating a Go champion was the headline.

Then in 2017 a paper came out.
"Attention Is All You Need."
It introduced the Transformer.
Nobody outside research cared at first.
Then GPT. Then GPT-2. Then GPT-3.
Then in November 2022, ChatGPT launched.
And the whole world changed in two months.

Now models write code. Like, real code.
They read repos like this one.
They suggest fixes. They open PRs.
This README itself was co-written with Claude.
In 2016 that sentence would get you laughed at.

### 🐙 GitHub grew up

When I pushed this, GitHub was scrappy.
Octocats. Indie energy. Free public repos.
Microsoft bought it in 2018.
Then came Actions, Codespaces, Copilot.
Now it has over 100 million developers.
And a decent chunk of commits involve AI help.
Including the one that brought this README in. 👋

---

## 🎞️ bonus: the 360 video detour

I can't tell this story without 360 video.
It was the sidekick of 2016 VR. 🦸

Back then, everyone wanted to film in 360.
Camera rigs looked like disco balls.
Six GoPros glued into a cube.
Sometimes sixteen.
You'd film a concert or a beach.
Then spend a week stitching the footage.
Seams everywhere. Ghost people at the edges. 👻
And the files? Enormous.

Then came the delivery problem.
A 360 video is a whole sphere.
To look sharp in a headset,
the full sphere needs to be huge.
Like, 8K-ish huge.
But you only ever see a slice.
About a fifth of the sphere, give or take.
So most of those bits go nowhere.
Streamed. Decoded. Never seen. 💸

```
      THE 360 WASTE PROBLEM 🌐

            ┌───────── full sphere streamed ─────────┐
            │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
            │░░░░░░░░░░░░┌──────────────┐░░░░░░░░░░░░│
            │░░░░░░░░░░░░│ 👁️ you look  │░░░░░░░░░░░░│
            │░░░░░░░░░░░░│  only this   │░░░░░░░░░░░░│
            │░░░░░░░░░░░░└──────────────┘░░░░░░░░░░░░│
            │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
            └────────────────────────────────────────┘
               ░ = paid for, never seen
```

So people tried everything.
Big platforms experimented with smarter projections.
Cubemaps instead of flat world-map layouts.
Pyramids that put detail in front of you.
Tiles that stream only the slice you face.
My other 2016 repo, the VR video compressor,
was a fork of FFmpeg doing exactly that kind of thing.
Turning world-map video into cubemaps.

Foveation was the next step after all that.
Don't just stream the slice you face.
Stream the spot inside the slice your eye hits.
Slice of a slice.
Bits only where the fovea lands.

What happened to 360 video after?
Honestly, it cooled off.
Most people don't want to turn around while watching.
They want a great frame, chosen for them.
Directors exist for a reason. 🎬
360 found homes in sports, real estate tours, training.
But it never became the default.

The idea underneath survived though.
"Spend bits where attention goes."
That's now everywhere.
In video calls that sharpen your face and blur the room.
In games that render your focus area in full.
In AI models that pay "attention" to the important tokens.
Even the Transformer paper is named after attention.
Same instinct. Different decade.

---

## 🧑‍🔬 bonus: what research figured out since

The science also grew up a lot.
A few things researchers learned that 2016 me didn't know.

**Eyes are fast. Like, really fast.** ⚡
A saccade is a quick jump of the eye.
It's over in a blink. Faster, actually.
If the sharp zone lags behind the eye,
you see the blur for a moment.
That's called a "pop".
And it's super noticeable.
So the system must be fast,
or it must predict where the eye will land.
Prediction won. Modern systems guess the landing spot.

**You're half-blind during a jump.** 🙈
During a saccade, your brain mutes your vision a bit.
It's called saccadic suppression.
That gives systems a tiny window to cheat.
Swap the sharp zone mid-jump.
You never see the switch.
Nature hands you free frames. 🎁

**Blur isn't all the same.** 🌫️
Plain blur at the edges looks wrong.
It feels like smearing Vaseline on the lens.
Research found the periphery cares about contrast and motion.
Keep the edges' contrast. Drop the fine detail.
Your brain stays happy.
Your GPU stays cool.

**Everyone's eyes differ.** 👓
Glasses wearers. Different pupil distances.
Lazy eyes. Tired eyes at 2am.
Calibration matters a lot.
Good systems calibrate in a few seconds now.
Look at a dot. Look at another dot. Done.

**Foveation helps networks too.** 📡
For cloud streaming of VR,
the server can encode around your gaze.
Fewer bits over the air.
Less lag. Less battery.
That's the "encoder" idea from this repo.
Researchers kept building on it for years.

None of this was in my 2016 code.
But all of it is in my 2026 plan.
That's what ten years buys you.
Not just better chips.
Better questions. 🔍

---

## 🥽 6. the headset timeline, 2016 → 2026

Let me put the VR story in one picture.

```
 2016 ┤ 🥽 Oculus Rift CV1 ships        (tethered, PC, no eye tracking)
      │ 🎮 HTC Vive ships               (room-scale! lighthouses!)
      │ 🎮 PlayStation VR ships         (VR for console folks)
      │ 📱 phone-in-a-visor headsets    (cheap, cute, nausea included)
      │
 2018 ┤ 🥽 standalone headsets appear   (no PC needed)
      │ 🧮 GPUs get "variable rate shading" (foveation, in hardware)
      │
 2019 ┤ 🥽 Oculus Quest                 (standalone + 6DoF, big moment)
      │
 2020 ┤ 🥽 Quest 2                      (cheap enough for normal people)
      │ 😷 pandemic → headsets fly off shelves
      │
 2021 ┤ 🏢 Facebook → Meta
      │
 2022 ┤ 👁️ Quest Pro                    (eye tracking, finally, on Quest)
      │
 2023 ┤ 👁️ PS VR2                       (eye-tracked foveated rendering)
      │ 🥽 Quest 3                      (colour passthrough, mixed reality)
      │ 👓 Ray-Ban Meta glasses         (camera + AI on your face)
      │
 2024 ┤ 🍎 Apple Vision Pro             (gaze IS the cursor)
      │ 👓 AR glasses prototypes shown  (true holograms, not for sale)
      │
 2025 ┤ 👓 display glasses ship         (a tiny screen in the lens)
      │ 🤖 Android XR headsets          (Google is back)
      │
 2026 ┤ 👓 AI glasses everywhere        (and SeeIt is building open ones)
      ▼
```

Look at where eye tracking sits.
2016: nowhere.
2022 onward: in the premium stuff.
2024: it's the whole interface on Vision Pro.

That's the arc this repo was guessing at.
It just guessed six years early. 😅
Being early is the same as being wrong.
Until it isn't.

---

## 🧮 7. the compute timeline

Foveation exists because compute is scarce.
So let's see how compute changed.

**2016.** 🖥️
The GTX 1080 launched that year.
A monster card for its time.
VR needed a "VR ready" PC.
That label was a real marketing thing.
Most laptops couldn't do it at all.

**2018.** ⚡
Nvidia's Turing GPUs shipped.
They added variable rate shading.
In plain words: the GPU itself can shade
some parts of the screen in less detail.
That's foveation baked into silicon.
What this repo did in custom code,
the hardware now did for free-ish.

**2019 to 2022.** 📱
Mobile chips got scary good.
Standalone headsets run on phone-class chips.
Qualcomm's XR chips power most of them.
Foveation becomes mandatory, not optional.
Without it, mobile chips can't keep up.

**2023 to 2026.** 🧠
GPUs stop being "for games".
They're for AI now.
Data centres hoard them.
Prices go up.
The irony is thick. 🍯
The chips VR needed got eaten by chatbots.
Which makes efficiency matter even more.

```
   WHY FOVEATION KEEPS WINNING 🏆

   pixels needed  ──────►  ↗↗↗  (higher res screens every year)
   chip budget    ──────►  →→→  (battery & heat say no)
                              │
                              ▼
               the gap keeps growing
                              │
                              ▼
          👁️ spend pixels only where the eye looks
```

The gap never closes.
Screens get sharper faster than chips get cooler.
So the trick from 2016 is still the trick.
Just smarter now.

---

## 🤖 8. the AI plot twist nobody saw coming

This twist is my favourite.
In 2016, foveation was about *saving* pixels.
In 2026, AI *invents* pixels.

Think about that.

Neural upscalers take a blurry frame.
They guess the sharp version.
And they guess well.
Games ship this as a standard setting now.
Render low. Let AI fill in the detail.

Now mix that with foveation.

```
     2016 FOVEATION                   2026 FOVEATION + AI
  ┌──────────────────┐            ┌──────────────────┐
  │ edges: low res   │            │ edges: tiny res  │
  │ centre: full res │   ────►    │ centre: low res  │
  │                  │            │  + 🤖 AI fills   │
  │ 🧮 saves ~half   │            │    in the detail │
  └──────────────────┘            │ 🧮 saves way more│
                                  └──────────────────┘
```

Render almost nothing.
Track the eye.
Let a small model paint the sharp bit.
Your brain fills in the rest, as always.
Two lies stacked on each other.
Both beautiful. 🧙🧙

And video went further.
In 2016, 360 video meant cameras with six lenses.
Stitching. Pain. Giant files.
In 2026, you type a sentence.
A model makes the video.
At ShopOS, our Sloosh platform does this for brands every day.
Text in. Video out.
Even the profile picture on my GitHub came from it.

So the question changes.
2016: "how do we stream fewer pixels?"
2026: "how do we generate only the pixels you'll see?"
That's a way better question.
And foveation is the answer to both.

---

## 🛠️ 9. what i'd build now

Okay. If I started this repo today.
Fresh. No 2016 baggage.
The plan looks like this.

### 🎯 the goal

A tiny open-source library.
Foveated encoding for AI glasses.
Runs on a phone-class chip.
Works with any eye tracker.
Pairs with a small AI upscaler.

### 🏗️ the architecture

```
     👁️ eye tracker                 🎥 camera / video / game frame
         │                                   │
         ▼                                   ▼
  ┌─────────────┐                   ┌──────────────────┐
  │ gaze filter │ ← smooth the       │ region splitter  │
  │  (tiny ML)  │   jittery eye      │ (rings around    │
  └──────┬──────┘   data             │  the gaze point) │
         │                           └────────┬─────────┘
         └──────────────┬─────────────────────┘
                        ▼
              ┌───────────────────┐
              │ budget planner 🎯 │  ← battery, heat, network
              └─────────┬─────────┘
                        ▼
         ┌──────────────┴──────────────┐
         ▼                             ▼
  ┌─────────────┐              ┌──────────────┐
  │ encode hi-q │              │ encode low-q │
  │  (centre)   │              │   (edges)    │
  └──────┬──────┘              └──────┬───────┘
         └──────────────┬─────────────┘
                        ▼
              ┌───────────────────┐
              │ 🤖 AI upscaler     │  ← paints detail back in
              │   (on device)     │
              └─────────┬─────────┘
                        ▼
                    👓 your eye
```

### 🧰 the stack

- **Rust or C++** for the core. Speed matters.
- **WebGPU** for a demo anyone can open in a browser.
- **A tiny gaze predictor.** Eyes jump fast. Predict where they land.
- **Modern codecs.** AV1 handles region quality well.
- **A small upscaler model.** Quantised. On device.
- **Python bindings.** So researchers can play.

### 🧪 the demo

Open a web page.
Turn on your webcam.
Webcam guesses where you look. Rough, but fun.
The page renders a scene.
Sharp where you look. Blurry elsewhere.
A counter shows bits saved, live. 📉
Shareable. Weird. Delightful.
That's the kind of demo that spreads.

### 🔗 the SeeIt tie-in

At SeeIt we're building open AI glasses.
They need exactly this.
Battery life is the whole game on glasses.
So the 2026 version of this repo
likely lives inside SeeIt's stack.
That's the nicest kind of revival.
Not a museum piece.
A part in a new machine. ⚙️

---

## 📚 10. lessons from a ten-year-old repo

I re-read this code with a cup of chai.
Seven lessons fell out. ☕

**1. Good ideas wait for their hardware.** ⏳
Foveation was right in 2016.
It needed eye tracking to be cheap.
That took six years.
If your idea is right but early, write it down.
Push the repo anyway. Future you will thank you.

**2. Small modes beat one big program.** 🧩
The runmode pattern still holds up.
Each mode is a tiny test of one idea.
Today I'd call these skills, or agents, or tools.
Same thing. Small pieces. Loosely joined.

**3. Don't marry one SDK.** 💍
This code is glued to an old Oculus SDK.
That SDK is long gone.
The idea survived. The glue didn't.
Wrap the vendor stuff. Keep your core clean.

**4. READMEs age faster than code.** 📜
The old README promised to "watch the world get compressed".
Bless that kid. 😂
Write what it does. Not what it dreams.

**5. Biology is the best spec sheet.** 🧬
The eye solved this problem millions of years ago.
We just copied it.
When stuck, ask how nature already does it.

**6. Efficiency never goes out of style.** 🌱
Every era thinks compute is basically free now.
Every era is wrong.
In 2016 it was VR. In 2026 it's AI.
Doing less, smartly, always wins.

**7. Open source is a time capsule.** 💊
This repo sat for ten years.
Nobody touched it. It didn't care.
And now it's a story.
Push your weird stuff. Make it public.
It becomes a record of who you were.

---

## 🗺️ the 2016 vs 2026 cheat sheet

| | 2016 🦕 | 2026 🚀 |
|---|---|---|
| 🥽 headsets | tethered to a big PC | standalone, chip inside |
| 👁️ eye tracking | research labs only | premium headsets, standard |
| 👓 smart glasses | a punchline | a real product category |
| 🧮 foveation | custom code like this repo | built into GPUs and OSes |
| 🎥 360 video | six-lens rigs + stitching | AI can generate it from text |
| 🤖 AI | is it a hot dog? 🌭 | writes code, makes video |
| 🐙 GitHub | indie | Microsoft + Copilot + Actions |
| 👨‍💻 writing this README | me, alone, at 2am | me + Claude, at 2am 😂 |

Some things never change.
It's still 2am. 🌙

---

## 🏃 how to run the 2016 code (if you're brave)

Honest warning: this might not build anymore.
The Oculus SDK it expects is ancient.
CUDA and OpenCV have moved on a lot.

If you want to try:

```bash
# you'll need: CMake, CUDA, OpenCV, an old Oculus SDK 🙏
git clone https://github.com/saitec/Foveated-Encoder-for-VR
cd Foveated-Encoder-for-VR
mkdir build && cd build
cmake ..
make
./foveate playvideo your_video.mp4   # start small
```

If it builds first try, message me.
I owe you a coffee. ☕
And a medal. 🏅

---

## 🙋 who should care about this in 2026

Quick list. If you're one of these, this matters to you.

- 👓 **Glasses builders.** Battery is your whole life. Foveate everything.
- 🎮 **Game devs on headsets.** Eye-tracked rendering buys you frame time.
- 📡 **Cloud streaming folks.** Encode around the gaze. Save the network.
- 🤖 **On-device AI teams.** Pair foveation with small upscalers. Magic.
- 🎓 **Students.** This is a great first research project. Eyes are cheap to test.
- 🧓 **2016 VR veterans.** Come back. The hardware finally caught up. 🫶

## 💌 11. p.s.

To 2016 me:
You were early. That's okay.
The eye-tracking headsets came.
Apple made gaze the cursor.
Glasses got cameras and brains.
And you ended up building glasses yourself.

Keep pushing weird repos.
Some of them come back. 🔁

To you, reading this:
If you're working on foveation, gaze, glasses, or tiny on-device AI,
come say hi.
I'm [@5aitec](https://x.com/5aitec).
Or [5aitec.com](https://5aitec.com).

---

<div align="center">

```
      👁️  see sharp where it matters  👁️
   ─────────────────────────────────────
     pushed 2016 · revived 2026 · still curious
```

*part of the [GitHub Run](https://github.com/saitec/github-run): one fix, one insight, every day.*

</div>
