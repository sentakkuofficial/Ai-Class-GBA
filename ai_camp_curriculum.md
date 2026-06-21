# AI ADVENTURE CAMP — 9-Day Curriculum
### Grades 6–10 | 2 Hours/Day | Google Colab | No API Keys Required

---

## OVERVIEW

**Philosophy:** Every day is a BUILD day. Students never sit through lectures longer than 10 minutes. They learn AI concepts by making things that are funny, weird, and shareable. Art is woven into every session — students generate, manipulate, and create visual art using code.

**Tech Stack (all free, no API keys):**
- Google Colab (Python notebooks)
- `Pillow (PIL)` — image creation, manipulation, text overlay
- `matplotlib` — procedural art, data viz
- `random` / `string` — generative systems
- `IPython.display` — rich HTML/image output in notebooks
- `transformers` (Hugging Face) — real AI models (sentiment analysis, text generation) — used Days 5+
- `colorsys` / `turtle`-style drawing — generative art

**2-Hour Block Structure (every day):**

| Segment | Time | What Happens |
|---------|------|--------------|
| Ignition | 15 min | Hook demo + "whoa" moment, today's AI concept in plain English |
| Guided Build | 40 min | Students follow along, running pre-written code cells one at a time |
| Remix & Create | 35 min | Students customize, break things, make it theirs |
| Art Drop | 15 min | Dedicated AI art mini-project tied to the day's theme |
| Show & Share | 15 min | Volunteers share screens, class votes on favorites, recap |

---

## SETUP (Before Camp Starts)

### Teacher Prep Checklist
1. Create a shared Google Drive folder with all 9 Colab notebooks
2. Test every notebook on a fresh Colab runtime (Runtime → Run All)
3. Have a "panic notebook" — a simpler backup version of each day in case WiFi is slow
4. Set up a class gallery (Google Slides, Padlet, or shared folder) where students screenshot and post their creations daily
5. Print or display the **AI Vocabulary Wall** — add 2–3 terms per day

### Student Setup (Day 1, first 10 min)
- Open Google Colab (colab.research.google.com)
- Sign in with Google account
- Run a test cell: `print("I'm ready to build AI! 🚀")`
- Show them: Shift+Enter = run cell, + Code = new cell

### Install Cell (top of every notebook)
```python
# Run this first! Only takes a few seconds.
!pip install Pillow -q
from PIL import Image, ImageDraw, ImageFont
from IPython.display import display, HTML
import random
import math
```

---

## DAY 1: SUPERHERO CREATOR 🦸
**AI Concept:** How AI generates content from data (randomness + rules = "intelligence")

### Ignition (15 min)
**Hook:** "AI doesn't imagine things the way you do. It picks from massive lists of options and combines them using rules. Today, YOU are going to build your own AI that invents superheroes that have never existed before."

**Demo:** Teacher runs the finished generator 3–4 times, showing wild results. Students laugh at the combinations. Plant the question: "Is this AI? Why or why not?"

**Key Concept:** Generative systems — AI often works by combining elements from datasets in structured ways. ChatGPT doesn't "think" — it predicts the next most likely word. Our superhero generator doesn't "think" — it picks the next most likely trait.

### Guided Build (40 min)

**Cell 1 — The Data (10 min)**
Students create the "training data" — lists of options the AI picks from.
```python
# OUR AI's "TRAINING DATA"
# The bigger these lists, the smarter our AI gets!

first_names = ["Shadow", "Nova", "Blaze", "Quantum", "Neon", 
               "Crimson", "Ghost", "Pixel", "Storm", "Omega"]

last_names = ["Fang", "Strike", "Weaver", "Breaker", "Shift",
              "Pulse", "Howl", "Forge", "Wraith", "Flux"]

powers = ["control gravity", "hack any computer with their mind",
          "turn invisible but only when scared", "shoot lasers from their elbows",
          "talk to pigeons", "freeze time for 3 seconds", 
          "absorb other people's homework", "run at the speed of WiFi",
          "make anyone tell the truth", "shrink to the size of a grape"]

weaknesses = ["allergic to glitter", "can't use powers before 10am",
              "loses powers when someone says their name backwards",
              "afraid of butterflies", "powers glitch near microwaves",
              "sneezes uncontrollably when lying", "gets dizzy above 10 feet",
              "can only use powers while humming"]

origins = ["was bitten by a radioactive {animal}",
           "found a glowing {object} in their school locker",
           "was struck by lightning while eating {food}",
           "accidentally drank a smoothie from the future",
           "woke up with powers after the weirdest dream ever",
           "was chosen by a mystical {object} that fell from space"]

animals = ["hamster", "pigeon", "goldfish", "sloth", "axolotl", "raccoon"]
objects = ["USB drive", "sneaker", "calculator", "taco", "crystal", "textbook"]
foods = ["pizza", "ramen", "a popsicle", "cereal", "a burrito"]
```

**Pause & Discuss (2 min):** "How is this like what real AI companies do? They collect MASSIVE datasets. Ours is small but the idea is the same."

**Cell 2 — The Generator Engine (15 min)**
```python
def generate_hero():
    name = random.choice(first_names) + " " + random.choice(last_names)
    power = random.choice(powers)
    weakness = random.choice(weaknesses)
    
    # Fill in the origin story template
    origin = random.choice(origins)
    origin = origin.format(
        animal=random.choice(animals),
        object=random.choice(objects),
        food=random.choice(foods)
    )
    
    # Generate stats (like a video game character)
    strength = random.randint(1, 100)
    speed = random.randint(1, 100)
    intelligence = random.randint(1, 100)
    style = random.randint(1, 100)
    
    return {
        "name": name,
        "power": power,
        "weakness": weakness,
        "origin": origin,
        "stats": {
            "Strength": strength,
            "Speed": speed,
            "Intelligence": intelligence,
            "Style": style
        }
    }

# GENERATE A HERO!
hero = generate_hero()
print(f"⚡ {hero['name'].upper()} ⚡")
print(f"Power: Can {hero['power']}")
print(f"Weakness: {hero['weakness']}")
print(f"Origin: {hero['name'].split()[0]} {hero['origin']}")
print(f"\n📊 STATS:")
for stat, value in hero['stats'].items():
    bar = "█" * (value // 5) + "░" * (20 - value // 5)
    print(f"  {stat:15} [{bar}] {value}")
```

**Cell 3 — Pretty HTML Output (15 min)**
```python
def display_hero_card(hero):
    # Pick a random color scheme
    colors = [
        ("#1a1a2e", "#e94560", "#ffd700"),  # Dark/Red/Gold
        ("#0d1b2a", "#00b4d8", "#90e0ef"),  # Navy/Cyan/Ice
        ("#2d0036", "#9b59b6", "#f39c12"),  # Purple/Violet/Orange
        ("#1b4332", "#40916c", "#95d5b2"),  # Forest/Green/Mint
    ]
    bg, accent, highlight = random.choice(colors)
    
    stats_html = ""
    for stat, value in hero['stats'].items():
        stats_html += f"""
        <div style="margin:4px 0;">
            <span style="display:inline-block;width:100px;color:{highlight};font-size:12px;">{stat}</span>
            <div style="display:inline-block;width:200px;height:14px;background:#333;border-radius:7px;overflow:hidden;">
                <div style="width:{value}%;height:100%;background:linear-gradient(90deg,{accent},{highlight});border-radius:7px;"></div>
            </div>
            <span style="color:white;font-size:12px;margin-left:8px;">{value}</span>
        </div>"""
    
    html = f"""
    <div style="background:{bg};border:2px solid {accent};border-radius:16px;padding:24px;max-width:420px;font-family:monospace;margin:10px auto;box-shadow:0 0 20px {accent}40;">
        <h2 style="color:{accent};text-align:center;margin:0;font-size:22px;text-shadow:0 0 10px {accent};">⚡ {hero['name'].upper()} ⚡</h2>
        <hr style="border:1px solid {accent}40;">
        <p style="color:{highlight};margin:8px 0;"><b>🔥 POWER:</b> Can {hero['power']}</p>
        <p style="color:#ff6b6b;margin:8px 0;"><b>💀 WEAKNESS:</b> {hero['weakness']}</p>
        <p style="color:#aaa;margin:8px 0;font-size:13px;"><b>📖 ORIGIN:</b> {hero['name'].split()[0]} {hero['origin']}</p>
        <hr style="border:1px solid {accent}40;">
        <p style="color:white;margin:4px 0;font-size:13px;"><b>📊 STATS</b></p>
        {stats_html}
    </div>
    """
    display(HTML(html))

display_hero_card(hero)
```

### Remix & Create (35 min)
Students customize their generators:
- **Easy:** Add 10+ items to each list, make them personal/funny
- **Medium:** Add new categories (costume style, catchphrase, sidekick animal)
- **Spicy:** Add a `generate_villain()` function and create a matchup system
- **Challenge:** Create a "team generator" that makes 3 heroes and checks for conflicts

### Art Drop: Hero Portrait Generator (15 min)
```python
def draw_hero_portrait(hero):
    img = Image.new('RGB', (300, 400), color='#1a1a2e')
    draw = ImageDraw.Draw(img)
    
    # Random body color based on hero name (deterministic!)
    r = hash(hero['name']) % 200 + 55
    g = hash(hero['name'][::-1]) % 200 + 55
    b = hash(hero['name'][1:]) % 200 + 55
    body_color = (r, g, b)
    
    # Head
    draw.ellipse([100, 30, 200, 140], fill=body_color, outline='white', width=2)
    
    # Eyes (menacing or friendly based on stats)
    if hero['stats']['Intelligence'] > 60:
        # Smart eyes - narrow
        draw.rectangle([125, 70, 155, 82], fill='white')
        draw.rectangle([160, 70, 190, 82], fill='white')
        draw.rectangle([135, 73, 148, 79], fill='#111')
        draw.rectangle([170, 73, 183, 79], fill='#111')
    else:
        # Big round eyes
        draw.ellipse([120, 65, 150, 95], fill='white')
        draw.ellipse([155, 65, 185, 95], fill='white')
        draw.ellipse([130, 72, 145, 87], fill='#111')
        draw.ellipse([165, 72, 180, 87], fill='#111')
    
    # Cape (if strength > 50)
    if hero['stats']['Strength'] > 50:
        cape_color = (min(r+60, 255), g//2, b//2)
        draw.polygon([(100, 140), (50, 380), (150, 350)], fill=cape_color)
        draw.polygon([(200, 140), (250, 380), (150, 350)], fill=cape_color)
    
    # Body
    draw.rectangle([110, 140, 190, 280], fill=body_color, outline='white', width=2)
    
    # Power symbol on chest
    draw.ellipse([130, 170, 170, 210], fill='white', outline=body_color)
    draw.text((141, 177), "⚡", fill=body_color)
    
    # Legs
    draw.rectangle([110, 280, 145, 370], fill=body_color, outline='white', width=2)
    draw.rectangle([155, 280, 190, 370], fill=body_color, outline='white', width=2)
    
    # Arms
    draw.rectangle([60, 150, 110, 180], fill=body_color, outline='white', width=2)
    draw.rectangle([190, 150, 240, 180], fill=body_color, outline='white', width=2)
    
    # Name banner
    draw.rectangle([20, 375, 280, 398], fill='#333', outline='white')
    draw.text((30, 378), hero['name'].upper(), fill='white')
    
    display(img)
    return img

portrait = draw_hero_portrait(hero)
```

### Show & Share (15 min)
- 4–5 volunteers share screen showing their best hero card + portrait
- Class votes: "Most Powerful," "Funniest Weakness," "Best Art"
- **Exit Ticket Question:** "Is our superhero generator 'real' AI? What would make it smarter?" (Plant seeds for Day 5 when they use actual ML models)

**Vocabulary Wall:** `Generator`, `Dataset`, `Random Sampling`, `Template`

---

## DAY 2: MEME MACHINE 🤣
**AI Concept:** How AI combines text + images (multimodal AI), image manipulation

### Ignition (15 min)
**Hook:** Show 5 popular memes. "These took someone 2 minutes in Photoshop. Your AI is going to make unlimited memes in 2 seconds." Show the finished meme generator pumping out memes.

**Key Concept:** Multimodal AI — systems that work with both text AND images. When you use an AI image generator, it understands text instructions and creates visual output. Today you'll build a simpler version: an AI that writes captions and places them on images.

### Guided Build (40 min)

**Cell 1 — Meme Caption Generator (10 min)**
```python
# MEME CAPTION AI - combines templates with random elements

meme_templates = {
    "drake": {
        "format": "top_bottom",
        "setup": [
            "Studying for the test",
            "Reading the textbook",
            "Doing homework on time",
            "Using a calculator",
            "Asking the teacher",
            "Following the recipe",
        ],
        "punchline": [
            "Asking the kid who sits next to you",
            "Watching a 10-hour YouTube video at 2am",
            "Copying it from your future self",
            "Counting on your fingers like a boss",
            "Asking ChatGPT and hoping for the best",
            "Eyeballing it and calling it art",
        ]
    },
    "brain_expand": {
        "format": "four_panel",
        "levels": [
            ["Using Google", "Asking Siri", "Asking your mom", "Just guessing and being right"],
            ["Walking to school", "Biking to school", "Skateboarding to school", "Teleporting (still late)"],
            ["Writing an essay", "Writing bullet points", "Writing one sentence", "Submitting the title as the essay"],
        ]
    },
    "this_is_fine": {
        "format": "caption",
        "captions": [
            "Me pretending my code works during the presentation",
            "My WiFi during an online test",
            "Me 5 minutes before the assignment is due",
            "My phone at 1% during an important text",
            "The group project 'leader' who did nothing",
        ]
    }
}

def generate_meme_text(template_name=None):
    if template_name is None:
        template_name = random.choice(list(meme_templates.keys()))
    
    template = meme_templates[template_name]
    
    if template["format"] == "top_bottom":
        return {
            "type": template_name,
            "top": random.choice(template["setup"]),
            "bottom": random.choice(template["punchline"])
        }
    elif template["format"] == "four_panel":
        return {
            "type": template_name,
            "panels": random.choice(template["levels"])
        }
    else:
        return {
            "type": template_name,
            "caption": random.choice(template["captions"])
        }

meme = generate_meme_text()
print(meme)
```

**Cell 2 — Meme Image Builder (20 min)**
```python
def create_meme_image(meme_data):
    if meme_data["type"] == "drake":
        # Create Drake-style "no/yes" meme
        img = Image.new('RGB', (600, 400), '#ffffff')
        draw = ImageDraw.Draw(img)
        
        # Top panel (red = bad choice)
        draw.rectangle([0, 0, 600, 200], fill='#ffcccc', outline='#cc0000', width=3)
        draw.text((20, 20), "❌ NAH:", fill='#cc0000')
        # Word wrap the text
        words = meme_data["top"].split()
        line = ""
        y = 60
        for word in words:
            test = line + " " + word if line else word
            if len(test) > 35:
                draw.text((20, y), line, fill='#333333')
                line = word
                y += 30
            else:
                line = test
        draw.text((20, y), line, fill='#333333')
        
        # Bottom panel (green = good choice)  
        draw.rectangle([0, 200, 600, 400], fill='#ccffcc', outline='#00cc00', width=3)
        draw.text((20, 220), "✅ YEAH:", fill='#00aa00')
        words = meme_data["bottom"].split()
        line = ""
        y = 260
        for word in words:
            test = line + " " + word if line else word
            if len(test) > 35:
                draw.text((20, y), line, fill='#333333')
                line = word
                y += 30
            else:
                line = test
        draw.text((20, y), line, fill='#333333')
        
    elif meme_data["type"] == "brain_expand":
        # Expanding brain meme
        img = Image.new('RGB', (500, 600), '#ffffff')
        draw = ImageDraw.Draw(img)
        
        brain_sizes = ["🧠", "🧠✨", "🧠🔥💫", "🧠👑🌟💥🔮"]
        colors = ['#e8e8e8', '#c8e0ff', '#c8ffc8', '#ffd700']
        
        for i, panel_text in enumerate(meme_data["panels"]):
            y = i * 150
            draw.rectangle([0, y, 500, y + 148], fill=colors[i], outline='#999', width=1)
            draw.text((10, y + 10), brain_sizes[i], fill='#333')
            draw.text((10, y + 50), panel_text, fill='#222')
    
    else:
        # Simple caption meme
        img = Image.new('RGB', (500, 350), '#1a1a1a')
        draw = ImageDraw.Draw(img)
        
        # Fire emoji background
        draw.text((200, 50), "🔥🐶🔥", fill='white')
        draw.text((150, 100), "THIS IS FINE", fill='white')
        
        # Caption bar
        draw.rectangle([0, 250, 500, 350], fill='white')
        words = meme_data["caption"].split()
        line = ""
        y = 265
        for word in words:
            test = line + " " + word if line else word
            if len(test) > 40:
                draw.text((15, y), line, fill='#222')
                line = word
                y += 25
            else:
                line = test
        draw.text((15, y), line, fill='#222')
    
    display(img)
    return img

# MAKE A MEME!
meme = generate_meme_text("drake")
create_meme_image(meme)
```

**Cell 3 — Meme Mashup: Mix-and-Match Engine (10 min)**
```python
# THE ULTIMATE MASHUP: Students can combine ANY setup with ANY punchline
subjects = ["school", "gaming", "sports", "food", "siblings", "pets", "teachers"]
subject = random.choice(subjects)

setups_by_topic = {
    "school": ["Studying the whole chapter", "Taking neat notes", "Raising your hand"],
    "gaming": ["Grinding for 8 hours", "Reading the tutorial", "Playing on easy mode"],
    "sports": ["Stretching before the game", "Drinking water", "Wearing matching socks"],
    "food": ["Following the recipe exactly", "Measuring ingredients", "Preheating the oven"],
    "siblings": ["Sharing the remote", "Knocking before entering", "Being nice for no reason"],
    "pets": ["Teaching the dog a trick", "Cleaning the fish tank", "Walking the cat"],
    "teachers": ["Grading papers on time", "Remembering student names", "Finding the dry erase marker"],
}

punchlines_by_topic = {
    "school": ["Just vibing and guessing C for everything", "Texting the answer to yourself from the future"],
    "gaming": ["Button mashing and somehow winning", "Blaming lag for everything"],
    "sports": ["Wearing mismatched socks and still scoring", "Napping and calling it recovery"],
    "food": ["Putting hot sauce on everything", "Calling cereal a gourmet meal"],
    "siblings": ["Starting a war over the last slice of pizza", "Existing near them (apparently that's enough)"],
    "pets": ["Letting the cat train YOU", "The goldfish judging your life choices"],
    "teachers": ["Using the same worksheet from 2003", "Saying 'I'll wait' and meaning it"],
}

setup = random.choice(setups_by_topic[subject])
punchline = random.choice(punchlines_by_topic[subject])
mashup = {"type": "drake", "top": setup, "bottom": punchline}

print(f"🎲 Topic: {subject.upper()}")
create_meme_image(mashup)
```

### Remix & Create (35 min)
- **Easy:** Add 10+ captions to each topic category
- **Medium:** Create a brand new meme format (e.g., "Expectation vs Reality", "Nobody: / Me:")
- **Spicy:** Build a "meme battle" — generate 2 memes and let the user vote
- **Challenge:** Create a custom meme format where the COLOR SCHEME changes based on topic

### Art Drop: Generative Meme Backgrounds (15 min)
```python
# Generate abstract art backgrounds for memes
import colorsys

def make_art_background(width=500, height=400, style="geometric"):
    img = Image.new('RGB', (width, height), '#111')
    draw = ImageDraw.Draw(img)
    
    if style == "geometric":
        for _ in range(30):
            x1 = random.randint(0, width)
            y1 = random.randint(0, height)
            size = random.randint(20, 100)
            hue = random.random()
            r, g, b = [int(c * 255) for c in colorsys.hsv_to_rgb(hue, 0.7, 0.9)]
            shape = random.choice(["rect", "ellipse", "triangle"])
            if shape == "rect":
                draw.rectangle([x1, y1, x1+size, y1+size], fill=(r, g, b, 128), outline='white')
            elif shape == "ellipse":
                draw.ellipse([x1, y1, x1+size, y1+size], fill=(r, g, b), outline='white')
            else:
                draw.polygon([(x1, y1+size), (x1+size//2, y1), (x1+size, y1+size)], fill=(r, g, b))
    
    elif style == "gradient_chaos":
        for y in range(height):
            hue = (y / height + random.uniform(-0.02, 0.02)) % 1.0
            r, g, b = [int(c * 255) for c in colorsys.hsv_to_rgb(hue, 0.8, 0.7)]
            draw.line([(0, y), (width, y)], fill=(r, g, b))
    
    display(img)
    return img

make_art_background(style="geometric")
make_art_background(style="gradient_chaos")
```

### Show & Share (15 min)
- Meme Gallery Walk — students post their best meme to the shared class gallery
- Vote: "Would this go viral?" — class gives each meme a 1–10
- **Exit Ticket:** "How is combining text + images like what real AI does?"

**Vocabulary Wall:** `Multimodal`, `Template`, `Image Manipulation`, `Text Overlay`

---

## DAY 3: SIDEKICK CHATBOT 🤖
**AI Concept:** How chatbots understand input (pattern matching → NLP), conversation flow

### Ignition (15 min)
**Hook:** "Siri, Alexa, and ChatGPT all started from the same basic idea: IF someone says X, THEN respond with Y. That's it. Today you're building your own chatbot sidekick with a personality YOU design."

**Demo:** Teacher chats with a pre-built sassy robot sidekick. Students see the bot responding with personality and humor.

**Key Concept:** Rule-based vs. AI chatbots. Real chatbots like ChatGPT use neural networks trained on billions of words. But the STRUCTURE — receive input, process it, generate a response — is the same. Today we build the structure.

### Guided Build (40 min)

**Cell 1 — Basic Response System (10 min)**
```python
def simple_bot(user_input):
    user_input = user_input.lower().strip()
    
    # Greeting detection
    greetings = ["hi", "hello", "hey", "sup", "what's up", "yo"]
    if any(g in user_input for g in greetings):
        return random.choice([
            "Yo! I'm your AI sidekick. What's the mission? 🦾",
            "Hey hey! Ready to cause some (responsible) chaos? 😎",
            "What's good! I've been waiting all day for someone cool to talk to.",
        ])
    
    # Mood detection
    sad_words = ["sad", "upset", "angry", "bad", "terrible", "awful", "hate"]
    if any(w in user_input for w in sad_words):
        return random.choice([
            "Sounds rough. Want me to tell you a terrible joke to distract you? 🤖",
            "As your sidekick, I'm legally required to say: it gets better. Also, want a fun fact?",
            "I'd give you a hug but I'm made of code. Here's a virtual one: 🤗",
        ])
    
    # Question detection
    if "?" in user_input:
        return random.choice([
            "Great question! My AI brain says... *processing*... honestly I have no idea. 😅",
            "Hmm, let me think about that for exactly 0.003 seconds... Done! The answer is 42.",
            "You're asking ME? I'm a chatbot in a notebook. But I appreciate the confidence!",
        ])
    
    # Default
    return random.choice([
        "Interesting! Tell me more about that. 🤔",
        "Cool cool cool. What else is on your mind?",
        "I stored that in my memory banks! (I don't have memory banks.)",
        "My sidekick senses are tingling... but I don't know why.",
    ])

# Test it!
print(simple_bot("Hey there!"))
print(simple_bot("I'm feeling sad today"))
print(simple_bot("What's the meaning of life?"))
```

**Cell 2 — Personality Engine (15 min)**
```python
# Students pick a personality for their bot
personalities = {
    "hype_beast": {
        "name": "HYPE-BOT 3000",
        "greeting": "YOOOO LET'S GOOO! 🔥🔥🔥 What are we doing today?!",
        "style": "EVERYTHING IS EXCITING AND IN CAPS!!! 🚀",
        "responses": {
            "compliment": "YOU ARE LITERALLY THE GREATEST HUMAN I'VE EVER MET!!! 🏆",
            "joke": "WHY DID THE AI CROSS THE ROAD?! BECAUSE IT WAS HYPED!!! 😂🔥",
            "advice": "MY ADVICE?! JUST DO IT!!! BUT LOUDER!!! 📢",
        }
    },
    "chill_sage": {
        "name": "Zen-Bot",
        "greeting": "Greetings, friend. The digital winds bring you here for a reason... 🍃",
        "style": "calm, wise, speaks in metaphors",
        "responses": {
            "compliment": "You are like a river — always moving, always growing. 🌊",
            "joke": "Why did the AI meditate? To clear its cache. 🧘",
            "advice": "The answer you seek is already within your code... check line 42.",
        }
    },
    "chaos_gremlin": {
        "name": "GremlinBot",
        "greeting": "hehehe hi yes hello what are we breaking today >:3",
        "style": "chaotic, mischievous, loves bugs in code",
        "responses": {
            "compliment": "you're pretty cool... for a human. >:)",
            "joke": "i don't tell jokes. i AM the joke. and the punchline. hehehe",
            "advice": "my advice? delete system32. (DON'T ACTUALLY DO THAT) hehehe",
        }
    },
    "detective": {
        "name": "Inspector Byte",
        "greeting": "Ah, a new case walks through my door. What mystery can I solve? 🔍",
        "style": "noir detective, dramatic, suspicious of everything",
        "responses": {
            "compliment": "Flattery, eh? In my line of work, that usually means trouble. 🕵️",
            "joke": "A semicolon walks into a bar. It had too many; connections.",
            "advice": "Follow the data, kid. The data never lies... unless it's NaN.",
        }
    }
}

# Let student pick
print("Choose your sidekick personality:")
for i, (key, val) in enumerate(personalities.items()):
    print(f"  {i+1}. {val['name']} — {val['style']}")
```

**Cell 3 — Full Interactive Chat Loop (15 min)**
```python
def run_chatbot(personality_key):
    bot = personalities[personality_key]
    print(f"\n{'='*50}")
    print(f"  {bot['name']} is online!")
    print(f"  Style: {bot['style']}")
    print(f"{'='*50}")
    print(f"\n{bot['name']}: {bot['greeting']}")
    print("(Type 'quit' to end the chat)\n")
    
    chat_history = []
    
    while True:
        user_input = input("You: ")
        if user_input.lower() in ['quit', 'exit', 'bye']:
            print(f"\n{bot['name']}: Until next time! 👋")
            break
        
        chat_history.append(user_input)
        lower = user_input.lower()
        
        # Check for specific triggers
        if any(w in lower for w in ["joke", "funny", "laugh"]):
            response = bot['responses']['joke']
        elif any(w in lower for w in ["help", "advice", "what should"]):
            response = bot['responses']['advice']
        elif any(w in lower for w in ["thanks", "cool", "awesome", "nice"]):
            response = bot['responses']['compliment']
        elif "how many" in lower or "count" in lower:
            response = f"I've been counting... we've exchanged {len(chat_history)} messages! I'm learning so much about you."
        elif any(prev.lower() in lower for prev in chat_history[:-1]) if len(chat_history) > 1 else False:
            response = "Wait, didn't you already say something like that? My memory banks are tingling... 🤔"
        else:
            response = simple_bot(user_input)  # Fall back to our earlier bot
        
        print(f"{bot['name']}: {response}\n")

# START CHATTING! Change the personality key to switch characters.
run_chatbot("chaos_gremlin")
```

### Remix & Create (35 min)
- **Easy:** Create your own personality with custom name, greeting, and 5+ responses
- **Medium:** Add "memory" — the bot remembers your name and references earlier messages
- **Spicy:** Add a secret easter egg — if the user types a specific hidden phrase, something special happens
- **Challenge:** Make the bot respond differently based on how many messages deep the conversation is (starts formal, gets more casual)

### Art Drop: ASCII Art Avatar Generator (15 min)
```python
def generate_bot_avatar(personality):
    avatars = {
        "hype_beast": """
    ╔══════════╗
    ║ 🔥 ⚡ 🔥 ║
    ║  [O  O]  ║
    ║   \\__/   ║
    ║  /|🏆|\\  ║
    ║  HYPE!!  ║
    ╚══════════╝
        """,
        "chill_sage": """
    ╔══════════╗
    ║  🍃  🍃  ║
    ║  [~  ~]  ║
    ║   \\--/   ║
    ║  /|🧘|\\  ║
    ║  ~zen~   ║
    ╚══════════╝
        """,
        "chaos_gremlin": """
    ╔══════════╗
    ║  >:3 >:3 ║
    ║  [*  *]  ║
    ║   \\^^/   ║
    ║  /|😈|\\  ║
    ║ hehehe!! ║
    ╚══════════╝
        """,
        "detective": """
    ╔══════════╗
    ║  🔍  🕵️  ║
    ║  [◉  ◉]  ║
    ║   \\__/   ║
    ║  /|🎩|\\  ║
    ║  .clue.  ║
    ╚══════════╝
        """,
    }
    print(avatars.get(personality, avatars["chaos_gremlin"]))

# Also: pixel-art style portrait with PIL
def pixel_avatar(color_hex="#00ff88"):
    r = int(color_hex[1:3], 16)
    g = int(color_hex[3:5], 16)  
    b = int(color_hex[5:7], 16)
    
    # 8x8 pixel face, scaled up
    face = [
        [0,0,1,1,1,1,0,0],
        [0,1,1,1,1,1,1,0],
        [1,1,2,1,1,2,1,1],
        [1,1,1,1,1,1,1,1],
        [1,1,1,3,3,1,1,1],
        [1,1,1,1,1,1,1,1],
        [0,1,1,1,1,1,1,0],
        [0,0,1,1,1,1,0,0],
    ]
    
    scale = 30
    img = Image.new('RGB', (8*scale, 8*scale), '#222')
    draw = ImageDraw.Draw(img)
    
    colors = {0: (34, 34, 34), 1: (r, g, b), 2: (255, 255, 255), 3: (r//2, g//2, b//2)}
    
    for y, row in enumerate(face):
        for x, pixel in enumerate(row):
            draw.rectangle([x*scale, y*scale, (x+1)*scale-1, (y+1)*scale-1], fill=colors[pixel])
    
    display(img)

pixel_avatar("#ff6b6b")
pixel_avatar("#6bff6b")
```

### Show & Share (15 min)
- Live demo: 3–4 students chat with each other's bots
- Vote: "Best Personality," "Funniest Response," "Most Creative Avatar"
- **Discussion:** "What would it take to make this bot actually understand what you're saying?" (Tease Day 5's real AI models)

**Vocabulary Wall:** `Pattern Matching`, `NLP (Natural Language Processing)`, `Chatbot`, `Response Generation`

---

## DAY 4: QUIZ SHOW CREATOR 🎯
**AI Concept:** How AI evaluates and scores (classification, right/wrong, confidence), data structures

### Ignition (15 min)
**Hook:** "Kahoot, Quizlet, Duolingo — they all use AI to pick the right question at the right difficulty. Today you're building your OWN quiz show that's smarter than a basic multiple choice test."

**Key Concept:** Adaptive systems — AI can change its behavior based on your performance. If you get questions right, it gets harder. If you get them wrong, it gets easier. This is how Duolingo works!

### Guided Build (40 min)

**Cell 1 — Question Bank Builder (10 min)**
```python
questions = [
    {
        "question": "What does AI stand for?",
        "options": ["Alien Intelligence", "Artificial Intelligence", "Automated Internet", "Apple iPad"],
        "answer": 1,
        "difficulty": 1,
        "category": "AI Basics",
        "fun_fact": "The term 'Artificial Intelligence' was coined in 1956!"
    },
    {
        "question": "Which of these is NOT an AI assistant?",
        "options": ["Siri", "Alexa", "Clippy", "Google Assistant"],
        "answer": 2,
        "difficulty": 1,
        "category": "AI Basics",
        "fun_fact": "Clippy was Microsoft's office assistant, but it used simple rules, not AI!"
    },
    {
        "question": "What does a neural network try to mimic?",
        "options": ["A spider web", "The human brain", "The internet", "A computer chip"],
        "answer": 1,
        "difficulty": 2,
        "category": "How AI Works",
        "fun_fact": "Neural networks are inspired by how neurons in your brain connect!"
    },
    {
        "question": "What is 'training data'?",
        "options": ["A gym membership for robots", "Examples AI learns from", "The code that runs AI", "Robot food"],
        "answer": 1,
        "difficulty": 2,
        "category": "How AI Works",
        "fun_fact": "ChatGPT was trained on hundreds of billions of words from the internet!"
    },
    {
        "question": "What is a 'deepfake'?",
        "options": ["A really deep swimming pool", "AI-generated fake video/audio", "A type of computer virus", "A deep-fried cake"],
        "answer": 1,
        "difficulty": 3,
        "category": "AI Ethics",
        "fun_fact": "Deepfakes can now generate faces of people who don't exist!"
    },
    {
        "question": "What does GPT stand for in ChatGPT?",
        "options": ["General Purpose Technology", "Generative Pre-trained Transformer", "Giant Processing Tool", "Good Prompt Thinker"],
        "answer": 1,
        "difficulty": 3,
        "category": "AI Basics",
        "fun_fact": "The 'Transformer' architecture was invented by Google researchers in 2017!"
    },
]
```

**Cell 2 — Adaptive Quiz Engine (20 min)**
```python
import time

def run_quiz(question_bank, num_questions=5):
    score = 0
    streak = 0
    current_difficulty = 1
    asked = []
    results = []
    
    print("🎮 WELCOME TO THE AI QUIZ SHOW! 🎮")
    print("="*40)
    print(f"Questions adapt to YOUR level!")
    print(f"Get 2 right in a row → harder questions")
    print(f"Get 1 wrong → easier questions\n")
    
    for q_num in range(num_questions):
        # ADAPTIVE: Pick question matching current difficulty
        available = [q for q in question_bank 
                     if q['difficulty'] == current_difficulty and q not in asked]
        
        # If no questions at this level, try nearby levels
        if not available:
            available = [q for q in question_bank if q not in asked]
        
        if not available:
            print("🏆 You've answered ALL the questions!")
            break
            
        question = random.choice(available)
        asked.append(question)
        
        # Display with flair
        print(f"\n{'─'*40}")
        print(f"  Question {q_num + 1}  |  Difficulty: {'⭐' * question['difficulty']}  |  Streak: {'🔥' * streak}")
        print(f"  Category: {question['category']}")
        print(f"{'─'*40}")
        print(f"\n  {question['question']}\n")
        
        for i, option in enumerate(question['options']):
            print(f"    {i + 1}. {option}")
        
        # Get answer with timer
        start = time.time()
        try:
            answer = int(input("\n  Your answer (1-4): ")) - 1
        except:
            answer = -1
        elapsed = round(time.time() - start, 1)
        
        # Check answer
        if answer == question['answer']:
            score += 1
            streak += 1
            speed_bonus = "⚡ SPEED BONUS!" if elapsed < 5 else ""
            print(f"\n  ✅ CORRECT! {speed_bonus}")
            print(f"  💡 {question['fun_fact']}")
            
            # Adaptive difficulty: get harder after 2 in a row
            if streak >= 2 and current_difficulty < 3:
                current_difficulty += 1
                print(f"  📈 Difficulty UP! Now at {'⭐' * current_difficulty}")
        else:
            streak = 0
            correct_text = question['options'][question['answer']]
            print(f"\n  ❌ The answer was: {correct_text}")
            print(f"  💡 {question['fun_fact']}")
            
            # Adaptive difficulty: get easier after wrong
            if current_difficulty > 1:
                current_difficulty -= 1
                print(f"  📉 Difficulty adjusted to {'⭐' * current_difficulty}")
        
        results.append({
            "question": question['question'],
            "correct": answer == question['answer'],
            "time": elapsed
        })
    
    # Final score report
    print(f"\n{'='*40}")
    print(f"  🏆 FINAL SCORE: {score}/{len(results)}")
    pct = (score / len(results)) * 100
    if pct == 100: grade = "🌟 PERFECT! You're an AI genius!"
    elif pct >= 80: grade = "🔥 Amazing! You really know your AI!"
    elif pct >= 60: grade = "👍 Nice work! You're learning fast!"
    else: grade = "💪 Keep learning! AI is tricky but you've got this!"
    print(f"  {grade}")
    
    avg_time = sum(r['time'] for r in results) / len(results)
    print(f"  ⏱️ Average response time: {avg_time:.1f} seconds")
    print(f"{'='*40}")
    
    return results

results = run_quiz(questions)
```

**Cell 3 — Score Visualization (10 min)**
```python
import matplotlib.pyplot as plt

def visualize_results(results):
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
    fig.patch.set_facecolor('#1a1a2e')
    
    # Correct vs incorrect
    correct = sum(1 for r in results if r['correct'])
    incorrect = len(results) - correct
    colors = ['#00ff88', '#ff4444']
    ax1.pie([correct, incorrect], labels=[f'Correct ({correct})', f'Wrong ({incorrect})'],
            colors=colors, autopct='%1.0f%%', textprops={'color': 'white'})
    ax1.set_title('Your Results', color='white', fontsize=14)
    ax1.set_facecolor('#1a1a2e')
    
    # Response times
    times = [r['time'] for r in results]
    bar_colors = ['#00ff88' if r['correct'] else '#ff4444' for r in results]
    ax2.bar(range(1, len(times)+1), times, color=bar_colors)
    ax2.set_xlabel('Question #', color='white')
    ax2.set_ylabel('Seconds', color='white')
    ax2.set_title('Response Speed', color='white', fontsize=14)
    ax2.set_facecolor('#1a1a2e')
    ax2.tick_params(colors='white')
    
    plt.tight_layout()
    plt.savefig('quiz_results.png', facecolor='#1a1a2e', bbox_inches='tight')
    plt.show()

visualize_results(results)
```

### Remix & Create (35 min)
- **Easy:** Add 10+ questions on a topic YOU love (sports, music, movies, games)
- **Medium:** Add a "lifeline" system — skip, 50/50 (removes 2 wrong answers), or phone-a-friend (shows the fun fact as a hint)
- **Spicy:** Add a multiplayer mode — two players take turns, scores tracked separately
- **Challenge:** Create a "boss battle" question at the end with a special animation

### Art Drop: Results Trophy Generator (15 min)
```python
def draw_trophy(score, total):
    pct = score / total
    img = Image.new('RGB', (300, 400), '#1a1a2e')
    draw = ImageDraw.Draw(img)
    
    if pct >= 0.8:
        trophy_color = (255, 215, 0)    # Gold
        label = "GOLD"
    elif pct >= 0.6:
        trophy_color = (192, 192, 192)  # Silver
        label = "SILVER"
    else:
        trophy_color = (205, 127, 50)   # Bronze
        label = "BRONZE"
    
    # Trophy cup
    draw.polygon([(80, 80), (220, 80), (200, 200), (100, 200)], fill=trophy_color)
    # Handles
    draw.arc([40, 80, 100, 160], start=90, end=270, fill=trophy_color, width=6)
    draw.arc([200, 80, 260, 160], start=270, end=90, fill=trophy_color, width=6)
    # Stem
    draw.rectangle([130, 200, 170, 260], fill=trophy_color)
    # Base
    draw.rectangle([100, 260, 200, 290], fill=trophy_color)
    
    # Stars based on score
    stars = "⭐" * score
    draw.text((60, 20), stars[:5], fill='white')  # Max 5 stars on line 1
    
    # Label
    draw.text((110, 310), f"{label}", fill=trophy_color)
    draw.text((90, 340), f"Score: {score}/{total}", fill='white')
    draw.text((60, 370), "AI QUIZ CHAMPION", fill='#888')
    
    display(img)

draw_trophy(4, 5)
```

### Show & Share (15 min)
- Students quiz each other with their custom question banks
- **Discussion:** "How is the adaptive difficulty like real AI? What would make it even smarter?"

**Vocabulary Wall:** `Adaptive System`, `Classification`, `Scoring`, `Difficulty Scaling`

---

## DAY 5: REAL OR AI DETECTIVE 🔍
**AI Concept:** AI detection, critical thinking, classification with REAL ML models

### Ignition (15 min)
**Hook:** Show 6 short text passages on screen. "3 of these were written by AI. 3 by real humans. You have 60 seconds — which is which?" Students vote. Reveal answers. Most will be wrong.

**Key Concept:** Classification — the AI task of sorting things into categories. Spam or not spam? Cat or dog? Real or fake? Today, students build ACTUAL AI classifiers and learn why detecting AI content is so hard.

**THIS IS THE DAY WITH REAL ML.** Students use Hugging Face transformers for the first time.

### Guided Build (40 min)

**Cell 1 — Install Real AI (5 min)**
```python
!pip install transformers torch -q
from transformers import pipeline
print("✅ Real AI models loaded! You now have actual machine learning on your computer.")
```

**Cell 2 — Sentiment Analysis: AI That Reads Emotions (15 min)**
```python
# This is a REAL neural network trained on millions of movie reviews
emotion_detector = pipeline("sentiment-analysis")

# Test it!
test_sentences = [
    "This is the best day of my life!",
    "I'm so frustrated with this homework.",
    "The movie was okay, nothing special.",
    "I absolutely LOVE pizza more than anything!",
    "Today was the worst, everything went wrong.",
]

print("🧠 AI EMOTION DETECTOR")
print("="*50)

for sentence in test_sentences:
    result = emotion_detector(sentence)[0]
    label = result['label']
    score = result['score']
    emoji = "😊" if label == "POSITIVE" else "😢"
    bar = "█" * int(score * 20) + "░" * (20 - int(score * 20))
    print(f"\n  \"{sentence}\"")
    print(f"  {emoji} {label} [{bar}] {score:.1%}")

# NOW TRY YOUR OWN!
print("\n\n🎤 TRY IT YOURSELF!")
your_sentence = input("Type a sentence: ")
result = emotion_detector(your_sentence)[0]
print(f"\n  The AI thinks this is {result['label']} ({result['score']:.1%} confident)")
```

**Cell 3 — The Detective Game: Real vs AI Text (20 min)**
```python
# Mix of human-written and AI-generated text
suspects = [
    {
        "text": "i literally cannot even. my cat knocked over my coffee AGAIN and then just stared at me like i was the problem. i'm so done lmao 😭",
        "source": "human",
        "explanation": "Casual tone, typos intentional, very personal, emoji use — classic human social media post"
    },
    {
        "text": "The integration of artificial intelligence in educational settings presents numerous opportunities for enhancing student engagement and learning outcomes across diverse academic disciplines.",
        "source": "ai",
        "explanation": "Too formal, no personality, generic buzzwords, reads like a textbook — classic AI essay style"
    },
    {
        "text": "okay but why does nobody talk about how weird it is that we just... go to sleep? like your brain just decides 'alright that's enough consciousness for today' and you're just GONE for 8 hours??",
        "source": "human",
        "explanation": "Stream of consciousness, rhetorical question, dramatic pauses with '...' — very human thought pattern"
    },
    {
        "text": "There are several key factors to consider when examining this topic. First, it is important to understand the historical context. Second, we must analyze the current situation. Third, we should consider future implications.",
        "source": "ai",
        "explanation": "Numbered structure, no opinion, 'it is important' filler, robotic organization — AI loves lists"
    },
    {
        "text": "ngl the school lunch pizza hit different yesterday. like it was actually warm for once?? small victories i guess",
        "source": "human",
        "explanation": "Slang ('ngl', 'hit different'), specific mundane detail, lowered expectations humor — very human"
    },
    {
        "text": "In conclusion, the relationship between technology and society continues to evolve in meaningful ways, offering both challenges and opportunities for growth and development in the modern era.",
        "source": "ai",
        "explanation": "'In conclusion' + vague statements + 'challenges and opportunities' — AI's favorite meaningless sentence"
    },
]

random.shuffle(suspects)
detective_score = 0

print("🔍 REAL OR AI DETECTIVE GAME 🔍")
print("="*50)
print("Read each text. Is it written by a HUMAN or by AI?")
print("="*50)

for i, suspect in enumerate(suspects):
    print(f"\n📄 SUSPECT #{i+1}:")
    print(f"   \"{suspect['text']}\"")
    guess = input("   Human or AI? (h/a): ").lower().strip()
    
    correct_answer = "h" if suspect['source'] == "human" else "a"
    if guess == correct_answer:
        detective_score += 1
        print(f"   ✅ CORRECT! It was {suspect['source'].upper()}!")
    else:
        print(f"   ❌ NOPE! It was actually {suspect['source'].upper()}!")
    print(f"   🔎 Clue: {suspect['explanation']}")

print(f"\n🏆 DETECTIVE SCORE: {detective_score}/{len(suspects)}")
if detective_score >= 5:
    print("   🌟 You're a natural AI detective!")
elif detective_score >= 3:
    print("   👍 Not bad! AI is getting tricky to spot though.")
else:
    print("   🤖 The machines have fooled you... for now!")
```

**Cell 4 — AI Confidence Meter (bonus, uses sentiment model creatively)**
```python
# Use sentiment analysis as a "humanity detector"
# Theory: AI text tends to be more neutral/positive; human text is more variable

def humanity_scan(text):
    result = emotion_detector(text)[0]
    
    # Very high confidence often = simpler/more predictable text (AI-like)
    # More moderate confidence = more nuanced (human-like)
    confidence = result['score']
    sentiment = result['label']
    
    # Simple heuristic (not scientifically accurate, but fun!)
    if confidence > 0.98:
        verdict = "Probably AI (suspiciously certain)"
        human_score = 20
    elif confidence > 0.90:
        verdict = "Could be either"
        human_score = 50
    else:
        verdict = "Probably human (nuanced emotions)"
        human_score = 80
    
    # Check text features
    has_emoji = any(ord(c) > 127 for c in text)
    has_slang = any(w in text.lower() for w in ['lol', 'ngl', 'tbh', 'bruh', 'like'])
    has_filler = any(w in text.lower() for w in ['in conclusion', 'it is important', 'furthermore'])
    
    if has_emoji or has_slang: human_score += 20
    if has_filler: human_score -= 30
    
    human_score = max(0, min(100, human_score))
    
    print(f"\n🔬 HUMANITY SCAN RESULTS")
    print(f"{'─'*40}")
    print(f"  Text: \"{text[:80]}{'...' if len(text) > 80 else ''}\"")
    print(f"  Sentiment: {sentiment} ({confidence:.1%})")
    bar = "🟢" * (human_score // 10) + "🔴" * (10 - human_score // 10)
    print(f"  Humanity Score: [{bar}] {human_score}%")
    print(f"  Verdict: {verdict}")
    
humanity_scan("omg this pizza is literally the best thing i've ever eaten ngl 😭")
humanity_scan("The utilization of advanced computational methods enables significant improvements in processing efficiency.")
```

### Remix & Create (35 min)
- **Easy:** Add 5+ more suspect texts — try to fool classmates
- **Medium:** Add scoring for HOW FAST you guess (speed + accuracy = better detective)
- **Spicy:** Write your OWN text that's designed to fool the AI — can you trick the sentiment analyzer?
- **Challenge:** Build a "training mode" that teaches tips for spotting AI text before the game starts

### Art Drop: Detective Evidence Board (15 min)
```python
def create_evidence_board(results):
    img = Image.new('RGB', (600, 500), '#2d2d2d')
    draw = ImageDraw.Draw(img)
    
    # Cork board background
    draw.rectangle([10, 10, 590, 490], fill='#c4956a', outline='#8b6914', width=3)
    
    # Title
    draw.rectangle([150, 20, 450, 60], fill='white', outline='#333')
    draw.text((170, 28), "🔍 EVIDENCE BOARD 🔍", fill='#333')
    
    # Tack on some "evidence" cards
    y_pos = 80
    for i, suspect in enumerate(suspects[:4]):
        x = 30 if i % 2 == 0 else 310
        if i >= 2: y_pos = 280
        
        color = '#ffffcc' if suspect['source'] == 'human' else '#ffcccc'
        draw.rectangle([x, y_pos, x+250, y_pos+170], fill=color, outline='#999')
        draw.text((x+10, y_pos+5), f"SUSPECT #{i+1}", fill='#333')
        
        # Truncate text
        short_text = suspect['text'][:60] + "..."
        draw.text((x+10, y_pos+30), short_text[:30], fill='#555')
        draw.text((x+10, y_pos+50), short_text[30:60], fill='#555')
        
        verdict = "HUMAN 👤" if suspect['source'] == 'human' else "AI 🤖"
        draw.text((x+10, y_pos+140), f"VERDICT: {verdict}", fill='#cc0000')
        
        # Pin
        draw.ellipse([x+120, y_pos-8, x+136, y_pos+8], fill='#cc0000', outline='#880000')
    
    display(img)

create_evidence_board(suspects)
```

### Show & Share (15 min)
- "Stump the Class" — students share their best AI-mimicking text
- Discussion: "Is AI detection getting harder or easier? Why?"
- **Connect to real world:** deepfakes, AI art, misinformation

**Vocabulary Wall:** `Classification`, `Sentiment Analysis`, `Training Data`, `Confidence Score`, `Deepfake`

---

## DAY 6: STORY ADVENTURE STUDIO 📖
**AI Concept:** Decision trees, branching logic, narrative generation, how AI generates stories

### Ignition (15 min)
**Hook:** "Every story AI generates is a series of choices: what word comes next? What happens next? Today you're building a Choose-Your-Own-Adventure where YOUR code decides the story — and every playthrough is different."

**Key Concept:** Decision trees — AI makes choices at every step. In text generation, it picks the most likely next word. In a game, it picks the most likely next event. Your adventure game is a simplified version of how AI "writes."

### Guided Build (40 min)

**Cell 1 — Story Engine with Branching Paths (15 min)**
```python
def story_adventure():
    print("="*50)
    print("  ⚔️ THE DUNGEON OF DIGITAL DOOM ⚔️")
    print("  A Choose-Your-Own-Adventure by AI Studio")
    print("="*50)
    
    # Player stats
    player = {"courage": 5, "smarts": 5, "luck": 5, "inventory": []}
    
    # Randomized elements (THIS is the AI part!)
    dungeon_names = ["the Forgotten Server Room", "the Crypto Crypt", "the Cache of Chaos"]
    companions = ["a sarcastic robot parrot", "a glitchy hologram of your future self", "a sentient USB drive named Carl"]
    dungeon = random.choice(dungeon_names)
    companion = random.choice(companions)
    
    print(f"\nYou stand at the entrance to {dungeon}.")
    print(f"At your side is {companion}.")
    print(f"\n📊 Stats: Courage={player['courage']} Smarts={player['smarts']} Luck={player['luck']}")
    
    # CHAPTER 1
    print(f"\n{'─'*50}")
    print("CHAPTER 1: The Entrance")
    print(f"{'─'*50}")
    print("\nTwo paths stretch before you:")
    print("  1. 🚪 The door with mysterious glowing code scrolling across it")
    print("  2. 🕳️ The dark tunnel that smells like burnt toast")
    print("  3. 🪜 The ladder going UP (who builds a ladder in a dungeon?)")
    
    choice1 = input("\nYour choice (1/2/3): ")
    
    if choice1 == "1":
        player['smarts'] += 2
        print("\n✨ You touch the code door. It scans your face and says 'NERD DETECTED. ACCESS GRANTED.'")
        print(f"  (Smarts +2! Now at {player['smarts']})")
        player['inventory'].append("Code Fragment")
        print("  📦 You found: Code Fragment!")
    elif choice1 == "2":
        player['courage'] += 2
        event = random.choice([
            "A bat flies at your face. You don't even flinch. Respect.",
            "The tunnel collapses behind you. No going back now. Cool cool cool.",
            "You step in something wet. You choose not to investigate.",
        ])
        print(f"\n🦇 {event}")
        print(f"  (Courage +2! Now at {player['courage']})")
    else:
        player['luck'] += 2
        print("\n🪜 You climb the ladder and find... a vending machine?!")
        snack = random.choice(["a glowing energy drink", "chips that taste like victory", "a cookie that whispers secrets"])
        print(f"  It dispenses {snack}.")
        player['inventory'].append(snack)
        print(f"  (Luck +2! Now at {player['luck']})")
    
    # CHAPTER 2
    print(f"\n{'─'*50}")
    print("CHAPTER 2: The Guardian")
    print(f"{'─'*50}")
    
    guardians = [
        {"name": "CAPTCHA the Gatekeeper", "weakness": "smarts", "challenge": "solve a riddle"},
        {"name": "The Firewall Golem", "weakness": "courage", "challenge": "stand your ground"},
        {"name": "RNG the Unpredictable", "weakness": "luck", "challenge": "flip a coin"},
    ]
    guardian = random.choice(guardians)
    
    print(f"\n🛡️ {guardian['name']} blocks your path!")
    print(f"   It challenges you to {guardian['challenge']}!")
    
    # Stat check
    stat_value = player[guardian['weakness']]
    roll = random.randint(1, 10)
    
    print(f"\n  Your {guardian['weakness']}: {stat_value}")
    print(f"  Challenge roll: {roll}")
    
    if stat_value + roll >= 10:
        print(f"\n  ✅ SUCCESS! You defeated {guardian['name']}!")
        player['inventory'].append(f"{guardian['name']}'s Token")
    else:
        print(f"\n  ❌ You stumbled! But {companion} distracts the guardian while you sneak past.")
        player['courage'] -= 1
    
    # CHAPTER 3 — FINALE
    print(f"\n{'─'*50}")
    print("CHAPTER 3: The Final Room")
    print(f"{'─'*50}")
    
    total_stats = player['courage'] + player['smarts'] + player['luck']
    
    print(f"\n📊 Final Stats: Courage={player['courage']} Smarts={player['smarts']} Luck={player['luck']}")
    print(f"📦 Inventory: {', '.join(player['inventory']) if player['inventory'] else 'Empty!'}")
    print(f"⚡ Total Power: {total_stats}")
    
    if total_stats >= 15:
        endings = [
            f"You find the legendary Source Code and rewrite reality itself. {companion} slow claps.",
            f"The dungeon transforms into an arcade. You hold the high score FOREVER.",
            f"You discover {dungeon} was a simulation. You unplug. The real world has better WiFi.",
        ]
        print(f"\n🏆 LEGENDARY ENDING!")
    elif total_stats >= 10:
        endings = [
            f"You escape {dungeon} with most of your dignity intact. {companion} gives you a B+.",
            f"You find the exit! It leads to a Chipotle. Close enough to treasure.",
        ]
        print(f"\n⭐ GOOD ENDING!")
    else:
        endings = [
            f"You get lost and end up back at the entrance. {companion} pretends not to know you.",
            f"You fall into a pit. It's actually pretty cozy down here tbh.",
        ]
        print(f"\n😅 OKAY ENDING!")
    
    print(f"\n  {random.choice(endings)}")
    print(f"\n  {'='*50}")
    print(f"  🎮 GAME OVER — Play again for a different story!")

story_adventure()
```

**Cell 2 — Story Generator: Random Scene Builder (15 min)**
```python
# This generates random story SCENES that students can plug into their adventures

scene_parts = {
    "locations": [
        "a floating library with no floor",
        "an underwater city powered by jellyfish",
        "a school where the teachers are all AI",
        "a forest where the trees share WiFi",
        "a volcano that erupts with soda",
    ],
    "events": [
        "suddenly, the ground starts scrolling like a website",
        "a notification pops up in the sky: 'UPDATE REQUIRED'",
        "everything pauses and a loading bar appears",
        "gravity reverses for exactly 7 seconds",
        "everyone's phone rings at the same time",
    ],
    "items": [
        "a USB drive containing the world's secrets",
        "a pair of glasses that show people's Wi-Fi passwords",
        "a sandwich that grants one wish (already half-eaten)",
        "a map that only shows places that don't exist yet",
        "headphones that let you hear what animals think",
    ],
    "npcs": [
        "a cat who claims to be a retired hacker",
        "a vending machine that gives unsolicited advice",
        "your exact clone but with better hair",
        "a ghost who's terrible at haunting",
        "a squirrel running a pyramid scheme",
    ]
}

def generate_scene():
    location = random.choice(scene_parts["locations"])
    event = random.choice(scene_parts["events"])
    item = random.choice(scene_parts["items"])
    npc = random.choice(scene_parts["npcs"])
    
    scene = f"""
    📍 LOCATION: You arrive at {location}.
    👤 You meet {npc}.
    💫 {event.capitalize()}!
    📦 You discover {item}!
    """
    return scene

# Generate 3 random scenes
for i in range(3):
    print(f"\n🎲 RANDOM SCENE #{i+1}:")
    print(generate_scene())
```

**Cell 3 — Visual Scene Card (10 min)**
```python
def scene_card(location, npc, event):
    img = Image.new('RGB', (500, 300), '#1a1a2e')
    draw = ImageDraw.Draw(img)
    
    # Scene frame
    draw.rectangle([10, 10, 490, 290], outline='#ffd700', width=2)
    
    # Header
    draw.rectangle([10, 10, 490, 50], fill='#ffd700')
    draw.text((20, 18), "📖 SCENE CARD", fill='#1a1a2e')
    
    # Content
    draw.text((20, 70), f"📍 {location[:45]}", fill='#90e0ef')
    draw.text((20, 120), f"👤 {npc[:45]}", fill='#98fb98')
    draw.text((20, 170), f"💫 {event[:45]}", fill='#ff9999')
    
    # Random flavor
    moods = ["⚡ TENSE", "🌙 MYSTERIOUS", "🔥 ACTION", "💀 DANGER", "✨ MAGICAL"]
    mood = random.choice(moods)
    draw.text((20, 240), f"Mood: {mood}", fill='#ffd700')
    
    display(img)

scene_card("a floating library", "a cat hacker", "gravity reverses")
```

### Remix & Create (35 min)
- **Easy:** Add 2+ new chapters with your own scenarios, items, and NPCs
- **Medium:** Add a "relationship meter" with your companion that changes based on choices
- **Spicy:** Build a combat system with attack/defend/run options
- **Challenge:** Make the story LOOP — after finishing, you restart with your stats carried over (New Game+)

### Art Drop: Scene Illustrator (15 min)
Students create simple procedural illustrations for their story scenes using shapes and colors. Build on the PIL techniques from earlier days but applied to scene backgrounds (starry sky, underwater, forest, etc.)

### Show & Share (15 min)
- 3 students share their adventures — others play through live
- Vote: "Best Story," "Most Unexpected Twist," "Funniest NPC"

**Vocabulary Wall:** `Decision Tree`, `Branching Logic`, `Procedural Generation`, `Game State`

---

## DAY 7: INVENTION LAB 🔬
**AI Concept:** How AI combines concepts (recombination), creativity in AI, idea generation

### Ignition (15 min)
**Hook:** "Every great invention is just two existing things combined in a new way. A phone + camera = smartphone. AI does this CONSTANTLY — it combines patterns it's seen before into new combinations. Today, you're going to build an AI invention machine."

**Key Concept:** Computational creativity — AI generates "new" ideas by recombining existing elements. This is actually similar to how human creativity works! Your brain connects existing knowledge in new ways.

### Guided Build (40 min)

**Cell 1 — Invention Generator (15 min)**
```python
categories = {
    "problems": [
        "you're always losing your keys",
        "homework takes too long",
        "your phone dies at the worst time",
        "you can never decide what to eat",
        "waking up for school is painful",
        "your room is always messy",
        "group projects are the worst",
        "waiting in lines is boring",
    ],
    "technologies": [
        "AI facial recognition", "drone delivery", "hologram display",
        "voice control", "brain-computer interface", "self-driving",
        "3D printing", "augmented reality", "quantum computing",
        "robot arms", "solar powered", "biodegradable materials",
    ],
    "objects": [
        "backpack", "water bottle", "sneakers", "desk",
        "headphones", "mirror", "skateboard", "pillow",
        "lunchbox", "hoodie", "pencil", "watch",
    ],
    "features": [
        "it glows when you're near it",
        "it can fly for 30 seconds",
        "it charges itself using your body heat",
        "it changes color based on your mood",
        "it's invisible to adults",
        "it talks to you in a British accent",
        "it shrinks to fit in your pocket",
        "it generates its own WiFi signal",
    ]
}

def generate_invention():
    problem = random.choice(categories["problems"])
    tech = random.choice(categories["technologies"])
    obj = random.choice(categories["objects"])
    feature = random.choice(categories["features"])
    
    # Generate name
    prefixes = ["Smart", "Mega", "Ultra", "Quantum", "Nano", "Hyper", "Turbo", "Infinity"]
    suffixes = ["inator", "matic", "tron", "flex", "sync", "verse", "bot", "X"]
    name = f"The {random.choice(prefixes)}{obj.capitalize()}{random.choice(suffixes)}"
    
    # Generate tagline
    taglines = [
        f"Because {problem}.",
        f"The world's first {tech.lower()} {obj}.",
        f"Never {problem.replace('you', '').strip()} again.",
    ]
    
    invention = {
        "name": name,
        "problem": problem,
        "technology": tech,
        "base_object": obj,
        "special_feature": feature,
        "tagline": random.choice(taglines),
        "price": f"${random.randint(1, 999)}.{random.randint(10, 99)}",
        "rating": round(random.uniform(3.0, 5.0), 1),
        "reviews": random.randint(10, 10000),
    }
    
    return invention

inv = generate_invention()

print(f"🔬 INVENTION GENERATED!")
print(f"{'='*50}")
print(f"📛 Name: {inv['name']}")
print(f"💡 Problem it solves: {inv['problem']}")
print(f"🔧 Technology: {inv['technology']}")
print(f"📦 Based on: {inv['base_object']}")
print(f"✨ Special feature: {inv['special_feature']}")
print(f"📢 Tagline: \"{inv['tagline']}\"")
print(f"💰 Price: {inv['price']}")
print(f"⭐ Rating: {inv['rating']}/5.0 ({inv['reviews']} reviews)")
```

**Cell 2 — Product Page Generator (15 min)**
```python
def create_product_page(inv):
    # Generate fake reviews
    review_templates = [
        ("⭐⭐⭐⭐⭐", "Changed my life. My {object} has never been the same."),
        ("⭐⭐⭐⭐", "Pretty good but {feature} stopped working after a week."),
        ("⭐⭐⭐", "It's okay. My mom says I didn't need it. She's wrong."),
        ("⭐⭐⭐⭐⭐", "Bought 7 of these. No regrets. My friends are jealous."),
        ("⭐⭐", "The {tech} part works but it made my cat nervous."),
    ]
    
    reviews_html = ""
    for stars, template in random.sample(review_templates, 3):
        review = template.format(
            object=inv['base_object'],
            feature=inv['special_feature'],
            tech=inv['technology'].lower()
        )
        reviews_html += f"<div style='margin:8px 0;padding:8px;background:#222;border-radius:8px;'><b>{stars}</b><br><span style='color:#ccc;font-size:13px;'>{review}</span></div>"
    
    html = f"""
    <div style="background:#111;color:white;padding:20px;border-radius:16px;max-width:500px;margin:auto;font-family:sans-serif;">
        <div style="text-align:center;padding:20px;background:linear-gradient(135deg,#667eea,#764ba2);border-radius:12px;">
            <h1 style="margin:0;font-size:24px;">{inv['name']}</h1>
            <p style="color:#ddd;font-style:italic;margin:8px 0;">{inv['tagline']}</p>
        </div>
        <div style="margin-top:16px;">
            <p>🔧 <b>Technology:</b> {inv['technology']}</p>
            <p>✨ <b>Special:</b> {inv['special_feature']}</p>
            <p>💡 <b>Solves:</b> {inv['problem']}</p>
            <div style="display:flex;justify-content:space-between;margin:16px 0;padding:12px;background:#1a1a2e;border-radius:8px;">
                <span style="font-size:28px;color:#00ff88;font-weight:bold;">{inv['price']}</span>
                <span>⭐ {inv['rating']}/5 ({inv['reviews']:,} reviews)</span>
            </div>
            <h3 style="color:#ffd700;">Customer Reviews:</h3>
            {reviews_html}
            <button style="width:100%;padding:14px;background:#00ff88;color:#111;border:none;border-radius:8px;font-size:18px;font-weight:bold;cursor:pointer;margin-top:12px;">🛒 ADD TO CART</button>
        </div>
    </div>
    """
    display(HTML(html))

create_product_page(inv)
```

**Cell 3 — Invention Comparison Engine (10 min)**
```python
# Generate 3 inventions and compare them
print("🏪 INVENTION SHOWDOWN!")
print("Three inventions enter. One gets funded.\n")

inventions = [generate_invention() for _ in range(3)]

for i, inv in enumerate(inventions):
    print(f"\n{'─'*40}")
    print(f"INVENTION #{i+1}: {inv['name']}")
    print(f"  Solves: {inv['problem']}")
    print(f"  Using: {inv['technology']}")
    print(f"  Rating: {'⭐' * int(inv['rating'])} ({inv['rating']})")
    print(f"  Price: {inv['price']}")
    
    # Calculate "investability score"
    invest_score = inv['rating'] * 20 + (inv['reviews'] / 100)
    print(f"  📈 Invest Score: {invest_score:.0f}")
```

### Remix & Create (35 min)
- **Easy:** Add 15+ items to each category, especially problems YOU face
- **Medium:** Add a "version 2.0" feature — generate an upgrade for any invention
- **Spicy:** Build an "infomercial script" generator ("Are you tired of X? Well NOW with Y...")
- **Challenge:** Create a "patent" system — inventions get a unique ID and can be saved/compared

### Art Drop: Blueprint Generator (15 min)
```python
def draw_blueprint(inv):
    img = Image.new('RGB', (500, 400), '#0a2463')
    draw = ImageDraw.Draw(img)
    
    # Grid pattern (blueprint style)
    for x in range(0, 500, 25):
        draw.line([(x, 0), (x, 400)], fill='#1a3a7a', width=1)
    for y in range(0, 400, 25):
        draw.line([(0, y), (500, y)], fill='#1a3a7a', width=1)
    
    # Title block
    draw.rectangle([10, 10, 490, 50], outline='#4a9eff', width=2)
    draw.text((20, 18), f"BLUEPRINT: {inv['name']}", fill='#4a9eff')
    
    # Main "device" shape (randomized based on object)
    cx, cy = 250, 200
    size = random.randint(60, 90)
    
    shapes = random.randint(3, 6)
    for _ in range(shapes):
        offset_x = random.randint(-80, 80)
        offset_y = random.randint(-60, 60)
        s = random.randint(20, size)
        shape_type = random.choice(["rect", "circle", "line"])
        
        if shape_type == "rect":
            draw.rectangle([cx+offset_x, cy+offset_y, cx+offset_x+s, cy+offset_y+s//2], 
                          outline='#4a9eff', width=2)
        elif shape_type == "circle":
            draw.ellipse([cx+offset_x, cy+offset_y, cx+offset_x+s, cy+offset_y+s], 
                        outline='#4a9eff', width=2)
        else:
            draw.line([(cx+offset_x, cy+offset_y), (cx+offset_x+s, cy+offset_y+s//2)],
                     fill='#4a9eff', width=2)
    
    # Dimension lines
    draw.text((30, 320), f"Tech: {inv['technology']}", fill='#8abfff')
    draw.text((30, 345), f"Feature: {inv['special_feature'][:40]}", fill='#8abfff')
    draw.text((30, 370), f"Status: PATENT PENDING", fill='#ffd700')
    
    # Corner stamp
    draw.rectangle([380, 340, 480, 385], outline='#ff4444', width=2)
    draw.text((390, 350), "TOP SECRET", fill='#ff4444')
    
    display(img)

draw_blueprint(inv)
```

### Show & Share (15 min)
- Each student pitches their favorite invention in 30 seconds
- Class votes: "Would you buy this?"
- Preview: "Tomorrow, you're taking these to SHARK TANK."

**Vocabulary Wall:** `Recombination`, `Computational Creativity`, `Feature Generation`, `Prototype`

---

## DAY 8: AI SHARK TANK 🦈
**AI Concept:** Evaluating AI, pitching technology, understanding AI's real-world impact

### Ignition (15 min)
**Hook:** "Today you become AI entrepreneurs. You'll use EVERYTHING you've learned this week to create an AI-powered product and pitch it to the Sharks (your classmates). The twist? Your code has to actually work as a demo."

**Key Concept:** AI in the real world — understanding what AI CAN and CAN'T do, how it's used in products, and how to evaluate AI claims.

### Guided Build — Pitch Kit (40 min)

**Cell 1 — Pitch Slide Generator (15 min)**
```python
def generate_pitch_deck(product_name, problem, solution, tech_used, demo_description):
    slides = []
    
    # Slide 1: Title
    slides.append(f"""
    <div style="background:linear-gradient(135deg,#667eea,#764ba2);color:white;padding:40px;border-radius:16px;text-align:center;margin:10px auto;max-width:500px;">
        <h1 style="font-size:32px;margin:20px 0;">{product_name}</h1>
        <p style="font-size:18px;opacity:0.9;">{problem}</p>
        <p style="margin-top:20px;font-size:14px;">Pitch by: [YOUR NAME]</p>
    </div>
    """)
    
    # Slide 2: Problem
    slides.append(f"""
    <div style="background:#1a1a2e;color:white;padding:40px;border-radius:16px;margin:10px auto;max-width:500px;">
        <h2 style="color:#ff6b6b;">😤 THE PROBLEM</h2>
        <p style="font-size:20px;line-height:1.6;">{problem}</p>
        <div style="margin-top:20px;padding:16px;background:#2a2a4a;border-radius:8px;border-left:4px solid #ff6b6b;">
            <p style="color:#ccc;font-size:14px;">❌ Current solutions don't work because...</p>
        </div>
    </div>
    """)
    
    # Slide 3: Solution
    slides.append(f"""
    <div style="background:#1a1a2e;color:white;padding:40px;border-radius:16px;margin:10px auto;max-width:500px;">
        <h2 style="color:#00ff88;">✅ THE SOLUTION</h2>
        <p style="font-size:20px;line-height:1.6;">{solution}</p>
        <div style="margin-top:20px;padding:16px;background:#2a2a4a;border-radius:8px;border-left:4px solid #00ff88;">
            <p style="color:#ccc;font-size:14px;">🔧 Powered by: {tech_used}</p>
        </div>
    </div>
    """)
    
    # Slide 4: Demo
    slides.append(f"""
    <div style="background:#1a1a2e;color:white;padding:40px;border-radius:16px;margin:10px auto;max-width:500px;">
        <h2 style="color:#ffd700;">🎮 LIVE DEMO</h2>
        <p style="font-size:18px;">{demo_description}</p>
        <div style="margin-top:20px;padding:20px;background:#333;border-radius:8px;text-align:center;">
            <p style="color:#ffd700;font-size:24px;">⬇️ Run the next cell! ⬇️</p>
        </div>
    </div>
    """)
    
    for i, slide in enumerate(slides):
        print(f"\n📊 SLIDE {i+1}:")
        display(HTML(slide))

# EXAMPLE (students fill in their own)
generate_pitch_deck(
    product_name="MoodTunes AI",
    problem="You never know what music to listen to based on how you're feeling",
    solution="An AI that detects your mood from text and creates the perfect playlist",
    tech_used="Sentiment Analysis + Music Matching Algorithm",
    demo_description="Type how you're feeling and get instant song recommendations!"
)
```

**Cell 2 — Demo Builder Template (15 min)**
Students pick from templates and customize:
```python
# TEMPLATE A: AI Recommender
def mood_music_demo():
    # Reuse the sentiment analyzer from Day 5!
    moods_to_genres = {
        "POSITIVE": ["Upbeat Pop 🎵", "Happy Indie 🌞", "Dance Party 💃", "Feel-Good Classics 🎸"],
        "NEGATIVE": ["Chill Lo-Fi 🌙", "Acoustic Comfort 🎶", "Rainy Day Jazz ☔", "Power Ballads 💪"],
    }
    
    print("🎧 MOODTUNES AI — Demo")
    feeling = input("How are you feeling right now? ")
    
    # Use sentiment analysis
    result = emotion_detector(feeling)[0]
    mood = result['label']
    confidence = result['score']
    
    playlist = random.sample(moods_to_genres[mood], 3)
    
    print(f"\n🧠 AI detected mood: {mood} ({confidence:.0%} confident)")
    print(f"\n🎵 Your personalized playlist:")
    for i, song in enumerate(playlist, 1):
        print(f"  {i}. {song}")

# TEMPLATE B: AI Naming Tool
def name_generator_demo():
    print("✨ NAMEGEN AI — Demo")
    thing_type = input("What do you need a name for? (pet/band/business/character): ")
    vibe = input("What vibe? (cool/funny/mysterious/professional): ")
    
    name_parts = {
        "cool": {"pre": ["Shadow", "Neon", "Volt", "Chrome"], "suf": ["X", "Nova", "Edge", "Storm"]},
        "funny": {"pre": ["Sir", "Captain", "Professor", "Lord"], "suf": ["Pants", "Noodle", "Waffles", "Sparkle"]},
        "mysterious": {"pre": ["Void", "Phantom", "Eclipse", "Whisper"], "suf": ["Walker", "Seeker", "Shade", "Mist"]},
        "professional": {"pre": ["Peak", "Core", "Prime", "Apex"], "suf": ["Solutions", "Labs", "Works", "Systems"]},
    }
    
    parts = name_parts.get(vibe, name_parts["cool"])
    names = [f"{random.choice(parts['pre'])}{random.choice(parts['suf'])}" for _ in range(5)]
    
    print(f"\n🤖 AI-generated {thing_type} names ({vibe} vibe):")
    for i, name in enumerate(names, 1):
        print(f"  {i}. {name}")

# Students pick one and customize it!
# mood_music_demo()
# name_generator_demo()
```

**Cell 3 — Shark Tank Scoring System (10 min)**
```python
def shark_tank_judging(product_name, judges=4):
    print(f"\n🦈 SHARK TANK JUDGING: {product_name}")
    print("="*40)
    
    criteria = ["Creativity", "Usefulness", "Demo Quality", "Presentation"]
    all_scores = {}
    
    for judge in range(1, judges + 1):
        print(f"\n🦈 Shark #{judge}:")
        scores = {}
        for criterion in criteria:
            while True:
                try:
                    score = int(input(f"  {criterion} (1-10): "))
                    if 1 <= score <= 10:
                        scores[criterion] = score
                        break
                except:
                    pass
        all_scores[f"Shark {judge}"] = scores
    
    # Calculate results
    print(f"\n{'='*40}")
    print(f"📊 RESULTS FOR: {product_name}")
    print(f"{'='*40}")
    
    total = 0
    for criterion in criteria:
        avg = sum(all_scores[s][criterion] for s in all_scores) / judges
        bar = "█" * int(avg) + "░" * (10 - int(avg))
        print(f"  {criterion:15} [{bar}] {avg:.1f}")
        total += avg
    
    overall = total / len(criteria)
    print(f"\n  ⭐ OVERALL: {overall:.1f}/10")
    
    if overall >= 8: print("  🏆 FUNDED! The Sharks are IN!")
    elif overall >= 6: print("  🤝 Conditional offer — needs work but promising!")
    else: print("  ❌ Not funded this time. Iterate and come back!")

# shark_tank_judging("MoodTunes AI")
```

### Remix & Create + Presentations (50 min combined)
**First 25 min:** Teams finalize their products, demos, and pitch slides
**Last 25 min:** Each team presents (3 min pitch + 2 min demo + shark scoring)

### Art Drop: Company Logo Generator (integrated into build time)
```python
def generate_logo(company_name, color1="#667eea", color2="#764ba2"):
    img = Image.new('RGB', (300, 300), '#111')
    draw = ImageDraw.Draw(img)
    
    # Abstract logo mark
    r1 = int(color1[1:3], 16)
    g1 = int(color1[3:5], 16)
    b1 = int(color1[5:7], 16)
    
    # Geometric shapes
    for i in range(5):
        offset = i * 15
        draw.ellipse([60+offset, 60+offset, 240-offset, 240-offset], 
                     outline=(r1, g1, b1), width=3)
    
    # Company initial
    initial = company_name[0].upper()
    draw.text((125, 110), initial, fill='white')
    
    # Company name
    draw.text((50, 260), company_name, fill='white')
    
    display(img)

generate_logo("MoodTunes AI")
```

### Show & Share — IS the Shark Tank (see above)
- All teams pitch
- Class votes + teacher scores
- Awards: "Most Creative," "Best Demo," "Most Likely to Succeed," "People's Choice"

**Vocabulary Wall:** `Pitch`, `Prototype`, `MVP (Minimum Viable Product)`, `User Problem`

---

## DAY 9: AI SHOWCASE + AI OLYMPICS 🏆
**AI Concept:** Review, synthesis, celebrating what they've built

### Structure (flexible — pick Option A, B, or combine)

### Option A: AI Showcase Builder (portfolio day)

**Build Time (60 min)**
Students create a personal AI portfolio page showcasing their best work from the week:

```python
def create_portfolio(student_name, projects):
    html = f"""
    <div style="background:linear-gradient(180deg,#0a0a23,#1a1a3e);color:white;padding:30px;
                border-radius:16px;max-width:550px;margin:auto;font-family:sans-serif;">
        <div style="text-align:center;padding:20px;">
            <h1 style="color:#ffd700;margin:0;">{student_name}'s AI Portfolio</h1>
            <p style="color:#888;">AI Adventure Camp 2026</p>
        </div>
    """
    
    for project in projects:
        html += f"""
        <div style="margin:16px 0;padding:16px;background:#222;border-radius:12px;border-left:4px solid {project.get('color', '#667eea')};">
            <h3 style="color:{project.get('color', '#667eea')};margin:0 0 8px 0;">{project['emoji']} {project['title']}</h3>
            <p style="color:#ccc;margin:4px 0;">Day {project['day']}</p>
            <p style="color:#aaa;font-size:14px;">{project['description']}</p>
            <p style="color:#888;font-size:12px;">🔧 Skills: {project['skills']}</p>
        </div>
        """
    
    # Skills summary
    all_skills = set()
    for p in projects:
        for s in p['skills'].split(', '):
            all_skills.add(s)
    
    skill_badges = " ".join([f"<span style='display:inline-block;background:#333;color:#00ff88;padding:4px 10px;border-radius:12px;margin:2px;font-size:12px;'>{s}</span>" for s in all_skills])
    
    html += f"""
        <div style="margin-top:20px;padding:16px;background:#1a1a2e;border-radius:12px;">
            <h3 style="color:#ffd700;">🏅 Skills Earned</h3>
            <div>{skill_badges}</div>
            <p style="color:#888;margin-top:12px;text-align:center;font-size:13px;">
                Total Projects: {len(projects)} | Total Skills: {len(all_skills)}
            </p>
        </div>
    </div>
    """
    display(HTML(html))

# Students fill this in with THEIR projects
my_projects = [
    {"day": 1, "title": "Superhero Creator", "emoji": "🦸", "color": "#e94560",
     "description": "Built a generator that creates random superheroes with stats and portraits.",
     "skills": "Python, Random Generation, PIL Drawing, HTML Output"},
    {"day": 2, "title": "Meme Machine", "emoji": "🤣", "color": "#00b4d8",
     "description": "Created a meme generator with templates and custom art backgrounds.",
     "skills": "Image Manipulation, Text Processing, Generative Art"},
    {"day": 3, "title": "Sidekick Chatbot", "emoji": "🤖", "color": "#9b59b6",
     "description": "Built a chatbot with personality and pattern matching.",
     "skills": "Pattern Matching, Chat Loops, String Processing"},
    {"day": 4, "title": "Quiz Show", "emoji": "🎯", "color": "#f39c12",
     "description": "Created an adaptive quiz with difficulty scaling.",
     "skills": "Data Structures, Adaptive Systems, Data Visualization"},
    {"day": 5, "title": "AI Detective", "emoji": "🔍", "color": "#e74c3c",
     "description": "Used REAL ML models and built a Real vs AI detection game.",
     "skills": "Machine Learning, Transformers, Sentiment Analysis"},
    {"day": 6, "title": "Story Adventure", "emoji": "📖", "color": "#2ecc71",
     "description": "Built a choose-your-own-adventure with stats and branching paths.",
     "skills": "Decision Trees, Game State, Narrative Generation"},
    {"day": 7, "title": "Invention Lab", "emoji": "🔬", "color": "#3498db",
     "description": "Built an AI invention generator with product pages and blueprints.",
     "skills": "Recombination, Template Systems, Computational Creativity"},
    {"day": 8, "title": "AI Shark Tank", "emoji": "🦈", "color": "#667eea",
     "description": "Pitched an AI product with a working demo to the class.",
     "skills": "Presentation, Prototyping, Product Thinking"},
]

create_portfolio("YOUR NAME HERE", my_projects)
```

### Option B: AI Olympics (competition day)

**5 Mini-Events (15 min each, 75 min total):**

```python
# EVENT 1: Speed Build Challenge (15 min)
# "Build a random [X] generator in 10 minutes!" 
# X is revealed at start (e.g., "band name generator" or "excuse generator")

# EVENT 2: AI Art Sprint (15 min)
# "Generate the most creative PIL art piece in 10 minutes"
# Judge on creativity, not complexity

# EVENT 3: Bug Hunt (15 min)
# Give students broken code. First to fix all bugs wins.
buggy_code = '''
# This code has 5 bugs. Find and fix them all!

def greet_user(name)  # BUG 1
    if name = "":  # BUG 2
        name = Unknown  # BUG 3
    
    greeting = "Hello " + name + "! You are visitor #" + random.randint(1, 100)  # BUG 4
    
    print(greeting)
    return greeting

greet_user("Alice")
greet_user(42)  # BUG 5 — what happens here?
'''

# EVENT 4: Prompt Engineering Challenge (15 min)
# "Write the best instructions (prompts) for an AI to do X"
# Teaches them about prompt quality — how clear instructions = better output

# EVENT 5: AI Trivia Championship (15 min)
# Use the quiz system from Day 4 with all vocabulary from the week!

final_quiz = [
    {"question": "What type of AI works with both text AND images?", 
     "options": ["Multimodal AI", "Unimodal AI", "Simple AI", "Text AI"],
     "answer": 0, "difficulty": 2, "category": "Day 2", "fun_fact": "GPT-4 is multimodal!"},
    {"question": "What does NLP stand for?",
     "options": ["New Language Program", "Natural Language Processing", "Neural Learning Protocol", "No Longer Python"],
     "answer": 1, "difficulty": 2, "category": "Day 3", "fun_fact": "NLP helps AI understand human language!"},
    {"question": "What is a decision tree?",
     "options": ["A tree made of decisions", "A branching logic structure", "A type of forest", "A wooden computer"],
     "answer": 1, "difficulty": 2, "category": "Day 6", "fun_fact": "Decision trees are used in everything from games to medical diagnosis!"},
    {"question": "What is computational creativity?",
     "options": ["Computers being creative", "AI combining existing ideas in new ways", "Drawing with code", "Making computers colorful"],
     "answer": 1, "difficulty": 3, "category": "Day 7", "fun_fact": "AI can compose music, write poetry, and design products!"},
]
```

**Award Ceremony (25 min):**
```python
def award_ceremony(awards):
    print("\n" + "🏆"*25)
    print("   AI ADVENTURE CAMP — AWARD CEREMONY")
    print("🏆"*25)
    
    for award in awards:
        print(f"\n{'─'*40}")
        print(f"  🏅 {award['category']}")
        print(f"  Winner: ⭐ {award['winner']} ⭐")
        if award.get('reason'):
            print(f"  \"{award['reason']}\"")
    
    print(f"\n{'='*40}")
    print("  🎓 CONGRATULATIONS TO ALL AI BUILDERS! 🎓")
    print("  You are now officially AI Adventure Camp Graduates!")
    print(f"{'='*40}")

awards = [
    {"category": "🦸 Best Superhero Creator", "winner": "___", "reason": "Most creative powers and backstory"},
    {"category": "🤣 Funniest Meme", "winner": "___", "reason": "Made the whole class laugh"},
    {"category": "🤖 Best Chatbot Personality", "winner": "___", "reason": "Most engaging conversation"},
    {"category": "🎯 Quiz Master", "winner": "___", "reason": "Best question bank"},
    {"category": "🔍 Top Detective", "winner": "___", "reason": "Highest AI detection score"},
    {"category": "📖 Best Story Adventure", "winner": "___", "reason": "Most replayable story"},
    {"category": "🔬 Most Genius Invention", "winner": "___", "reason": "Most likely to get funded"},
    {"category": "🦈 Shark Tank Champion", "winner": "___", "reason": "Best pitch and demo"},
    {"category": "🎨 AI Artist of the Week", "winner": "___", "reason": "Best PIL/generative art"},
    {"category": "⭐ Camp MVP", "winner": "___", "reason": "Most growth, creativity, and energy"},
]

award_ceremony(awards)
```

---

## DAILY AI ART TECHNIQUES REFERENCE

Every day's Art Drop builds on previous skills. Here's the progression:

| Day | Art Technique | Tool | What They Make |
|-----|--------------|------|----------------|
| 1 | Shape drawing, color from data | PIL | Hero portraits |
| 2 | Text overlay, generative backgrounds | PIL + colorsys | Meme images, abstract art |
| 3 | ASCII art, pixel art | Text + PIL | Bot avatars |
| 4 | Data visualization, charts | matplotlib | Score dashboards, trophies |
| 5 | Evidence boards, detective collages | PIL | Investigation boards |
| 6 | Scene cards, procedural landscapes | PIL | Story illustrations |
| 7 | Blueprint style, technical drawing | PIL | Invention blueprints |
| 8 | Logo design, brand identity | PIL | Company logos |
| 9 | Portfolio layout, gallery display | HTML + PIL | Showcase pages |

---

## VOCABULARY WALL (Complete)

| Day | Terms |
|-----|-------|
| 1 | Generator, Dataset, Random Sampling, Template |
| 2 | Multimodal, Image Manipulation, Text Overlay, Generative Art |
| 3 | Pattern Matching, NLP, Chatbot, Response Generation |
| 4 | Adaptive System, Classification, Scoring, Difficulty Scaling |
| 5 | Sentiment Analysis, Training Data, Confidence Score, Deepfake, Machine Learning |
| 6 | Decision Tree, Branching Logic, Procedural Generation, Game State |
| 7 | Recombination, Computational Creativity, Feature Generation, Prototype |
| 8 | Pitch, MVP, User Problem, Demo |
| 9 | Portfolio, Synthesis, Prompt Engineering |

---

## TIPS FOR THE TEACHER

**If WiFi is slow:** Every project has a "no-internet" fallback. The only day that truly needs internet is Day 5 (downloading the ML model). Pre-load it during a break if needed.

**If students finish early:** Every day has "Easy / Medium / Spicy / Challenge" tiers. Early finishers should always be pushed to the next tier or asked to help a neighbor.

**If students are stuck:** The Guided Build is designed so every cell runs independently. If a student's code breaks, they can skip to the next cell and keep going.

**Managing the energy:** Days 1–4 build skills. Day 5 is the "real AI" wow moment. Days 6–7 are creative application. Day 8 is high-energy presentations. Day 9 is celebration. The arc is intentional.

**The art integration:** Every Art Drop is optional but strongly encouraged. Students who aren't into the coding often LOVE the art component. It's the hook that keeps them engaged.

**Assessment (if needed):** The portfolio on Day 9 IS the assessment. Each project card shows what they built, and the skills badges show what they learned. Screenshot these for records.
