from typing import Any
import time, random, threading, re

STOP_EVENT = threading.Event()
PAUSE_EVENT = threading.Event()

SPAM_ACTIVE = False
SPAM_STARTED_AT = 0.0
LAST_PING_MS = None

from base_plugin import BasePlugin, HookResult, HookStrategy
from client_utils import run_on_queue, send_message

__id__ = "trol"
__name__ = "Trol Split"
__description__ = "trol, troluse, trolerr, trolrand, trolupper, trollower, trolcaps, trolleet, trolrandtpl, trolreal, trolreal2, spam, pause, resume, stop, help, trolstatus"
__author__ = "@garyplugi"
__version__ = "2.3.0"
__icon__ = "exteraPlugins/2"
__min_version__ = "11.12.0"

DEFAULT_DELAY_MS = 200
REAL_MS_PER_CHAR = 120
REAL_MIN_DELAY_MS = 120
REAL_MAX_DELAY_MS = 5000

TEMPLATES = {
    "a1": """ты дерьмище ебанное посмей тут только навернуть вновь ебучая паскудница ты же тут будешь ловить на постоянной основе телочка ебанная наиничтожная чернь ебанная что ты пытаешься слабак ебучий я тебе мать ебал слышишь девчонка ебанная ты тут не выживешь телочка ебанная я тебе тут ебало все перетрахаю паскудница ебучая чисто тебе сынку шалавы ебанному все уши через свою залупу намотаю дабы ты ебанная девчонка не теряла стабильность я тебе тут нахуй твои ебучие попытки на жизнь все исключу ты будешь тут ловить ебучая падаль чтобы ты не пыталась сделать мразина ебучая я твою мамашу ебал слышишь меня не отсоси тут ебучая мразь тебе же сынку паскудницы ебанной тут все ебало перетрахаю чтобы ты не старалась ебанная мразина ты нихуя не сможешь мне воспрепятствовать ебучая гниль ты же у меня будешь ловить тут в ебалище тоннами дерьма и пытаться противопоставить мне что либо говницо ебучее я тебе говорю в руки себя взял щегол ебучий ты что не вкурил что тебе тут пиздец""",
    "a2": """ты же нахуй винипух ебаный чебурашка ебливая никчемная шалава беззубая я тебе все твои ноги сломаю ты ебучий сын залупоглазого винипуха открыл рот на своего хозяина и бога, и ща ты будешь своим вонючим гнилым ртом, отсасывать мой богоподобным пенис, я переебу всю твою семейку говноблядочных уродцев, я достану гранотомет и разбомблю твой блядский домик с твоими родителями ввиде чебурашек, я своим церберовским пенисом раздраблю все твои конечности и конечности твоей семьи и знакомых, ты будешь гнить в переулке в мусорке и медленно подыхать, я твою тупоголовую мать убью своим церберовским королевским величайшим богоподобным хуем и размажу её клитор по стенке, я ей сломаю все зубы своим величайственным хуем что она заплачет за то что ты ебучий поганный шишковатый поносный скорлупастый выродок влез в долги и я тебя как коллектор отпиздил своим пенисом, ты будешь плакать после того как увидешь что твоя жирная залупоглазая кривоглазая матуха будет весеть на стенке полудохлая, ты ебучая мразь у которого батя ушел из дома чтоб не оказаться жертвой моего королевского пениса, я раздавлю тебя как ебаного гнома, закатаю тебя в сигарету вместе с твоей матухой, и скурю тебя как ебаную травку, я расщеплю все твои конечности на атомы, ты будешь рассыпаться как будто только что танос щелкнул своими фиолетовыми пальчиками, которые были в клиторе твоей жирной матухи потаскухи, я найду твоего тупоголового отца который покинул тебя чтоб не стать жертвой моего пениса, и выдавлю ему глаза что он будет молить о пощаде, я поставлю твою бабку на колени и помещу свой хуй прямо в её глазницу, у неё выпадут глаза, хотя я думаю что они ей итак не нужны ведь у неё было зрение -7, а вы тупые говноблядские блядочарадею отдали своего деда талибам в афган чтоб его расчленили по аллахски, вам вообще не стыдно ебаным смурфикам из приюта веселого кротика? пока вы все смеялись, я раздраблял твоей бабке глаза, и ставил её раком, потом ебал её в её гнилую пизду своим королевским и отполированным твоей жирной бабкой пенисом, ты ебучие отродье, спермобак, ты даже не сможешь дать мне отпора ведь ты ебучий прыщавый уродец с комплексами, ебучий мальчуган которому я выдавлю все прыщи моим пенисом, а потом зарежу всю твою семейку ебучих уродцев на твоих глазах, а потом во все выдавлю тебе глаза, раздраблю тебя на мелкие кусочки, замариную и ты будешь лежать в морозке до тех пор пока я не найду собак которым можно тебя ебаного скарлупастого говноблядского выблядка скормить, ты не сможешь уже дать отпор в мой величайший пенис, ведь я бог и пародитель всех миров, я залез к твоей маме в сон и задушил её во сне, она проснулась в ужасе что не смогла даже подвинуться, а дальше я гранотометом разъебал твою хату твоей косоногое матухе глаза замариновал и сделал маринад для огурцов хуйлан ты беззубый межзубье твоё хуем прострелил 30 кг спермы в твоё влагалище залил еблан ты пиздарылый я тебя доведу до края моста и ты упадешь нахуй с высоты одного киллометра я же твою мамзелину ебал потреблядский ты подзалупный башмак я же тебе гному ебаному все зубы выковыривать буду своими боговидными пальцами ты мне ноги целовать будешь отребье ебаное я же тебе твоего отца в закуску под пиво преврачу хуйлан ты затраханный винипух ты кучерявый ты же нихуя тут не сможешь полупидорас никчемный всю твою родословную связь я на хуе своем вертел нищук ты бездарный я же тебе все твои зубки маленькие сломаю об своё колено твой нос оторву нахуй уродец ебаный ты лилипут с башмаком на голове я же тебе твою жопу на уши натяну как резинку на волосы твоей мамзелины ротовыебанной ты придурок несчастный ты нахуя родился если я прям сейчас прям тут твою матуху буду об стенку трахать в зад я же тебе прыщавому придурку твои кишки в жопу тебе заталкаю я тебя своим боговидным агригатом нахуй захуярю же я тебе по голове с такой силой буду своим членом стучать что у тебя вся черепная коробка в твое туловище зайдет ты же бессмысленный идиот без фантазии""",
    "a3": """привет желаю тебе хорошего дня и отличного настроения 🌹""",
}


def resolve_template_or_text(raw_text: str) -> str:
    key = raw_text.strip().lower()
    return TEMPLATES.get(key, raw_text)


def parse_params(raw_text: str):
    text = raw_text.strip()
    delay_ms = DEFAULT_DELAY_MS
    jitter_pct = 0

    if "|" in text:
        text, tail = text.split("|", 1)
        text = text.strip()
        tail = tail.strip()

        m = re.search(r"(\d+)", tail)
        if m:
            try:
                delay_ms = max(0, int(m.group(1)))
            except:
                delay_ms = DEFAULT_DELAY_MS

        j = re.search(r"~(\d{1,2})", tail)
        if j:
            try:
                jitter_pct = max(0, min(50, int(j.group(1))))
            except:
                jitter_pct = 0

    for opt in re.findall(r"\[(.+?)\]", raw_text):
        o = opt.strip().lower()
        if o.isdigit():
            delay_ms = max(0, int(o))
        elif o.startswith("~") and o[1:].isdigit():
            jitter_pct = max(0, min(50, int(o[1:])))

    return text, delay_ms, jitter_pct


def parse_real2_params(raw_text: str):
    text = raw_text.strip()
    m = re.search(r"([0-9]*\.?[0-9]+)\s*-\s*([0-9]*\.?[0-9]+)", text)

    if m:
        try:
            a = float(m.group(1))
            b = float(m.group(2))
            min_mul, max_mul = (min(a, b), max(a, b))
        except:
            min_mul, max_mul = 0.5, 1.5
        text = (text[:m.start()] + text[m.end():]).strip()
    else:
        min_mul, max_mul = 0.5, 1.5

    min_mul = max(0.0, min_mul)
    max_mul = min(100.0, max_mul)
    return text, min_mul, max_mul


def parse_spam_args(raw_text: str, default_delay_ms=DEFAULT_DELAY_MS):
    s = (raw_text or "").strip()
    tokens = re.findall(r"\[([^\]]*)\]", s)

    nums = []
    jitter_pct = None

    for t in tokens:
        t = t.strip()
        if re.fullmatch(r"~\d+", t):
            try:
                jitter_pct = max(0, min(50, int(t[1:])))
            except:
                pass
        elif re.fullmatch(r"\d+", t):
            try:
                nums.append(int(t))
            except:
                pass

    count = 1
    delay_ms = default_delay_ms

    if len(nums) >= 1:
        n1 = nums[0]
        if len(nums) >= 2:
            n2 = nums[1]
            if n1 >= 1000 and n2 <= 1000:
                count, delay_ms = n2, n1
            else:
                count, delay_ms = n1, n2
        else:
            if n1 >= 1000:
                delay_ms = n1
            else:
                count = n1

    count = max(1, min(count, 500))
    delay_ms = max(0, min(delay_ms, 60000))

    if jitter_pct is None:
        jitter_pct = 0

    core = re.sub(r"\[[^\]]*\]", "", s).strip()
    m = re.match(r'^"(.*?)"|^\'(.*?)\'|^(\S+)', core)

    word = ""
    if m:
        for g in m.groups():
            if g is not None:
                word = g
                break

    return word, count, delay_ms, jitter_pct


def send_words(peer: int, words, delay_ms=DEFAULT_DELAY_MS, jitter_pct=0):
    global SPAM_ACTIVE

    for w in words:
        if STOP_EVENT.is_set():
            break

        while PAUSE_EVENT.is_set() and not STOP_EVENT.is_set():
            time.sleep(0.05)

        if STOP_EVENT.is_set():
            break

        send_message({"peer": peer, "message": w})

        k = 1.0 + (random.uniform(-jitter_pct, jitter_pct) / 100.0) if jitter_pct > 0 else 1.0
        time.sleep(max(0.0, (delay_ms * k) / 1000.0))

    SPAM_ACTIVE = False


def send_words_real(peer: int, words):
    global SPAM_ACTIVE

    for w in words:
        if STOP_EVENT.is_set():
            break

        while PAUSE_EVENT.is_set() and not STOP_EVENT.is_set():
            time.sleep(0.05)

        if STOP_EVENT.is_set():
            break

        send_message({"peer": peer, "message": w})
        delay_ms = max(REAL_MIN_DELAY_MS, min(REAL_MAX_DELAY_MS, len(w) * REAL_MS_PER_CHAR))
        time.sleep(delay_ms / 1000.0)

    SPAM_ACTIVE = False


def send_words_real2(peer: int, words, min_mul: float, max_mul: float):
    global SPAM_ACTIVE

    for w in words:
        if STOP_EVENT.is_set():
            break

        while PAUSE_EVENT.is_set() and not STOP_EVENT.is_set():
            time.sleep(0.05)

        if STOP_EVENT.is_set():
            break

        send_message({"peer": peer, "message": w})

        mult = random.uniform(min_mul, max_mul)
        delay_s = max(0.0, min(REAL_MAX_DELAY_MS / 1000.0, mult * len(w)))
        time.sleep(delay_s)

    SPAM_ACTIVE = False


def send_spam(peer: int, word: str, count: int, delay_ms: int, jitter_pct: int = 0):
    global SPAM_ACTIVE

    if not word or count <= 0:
        return

    words = [word] * count
    send_words(peer, words, delay_ms=delay_ms, jitter_pct=jitter_pct)
    SPAM_ACTIVE = False


def distort_word(word: str) -> str:
    if len(word) <= 3:
        return word

    chars = list(word)
    choice = random.choice(["swap", "drop", "dup"])

    if choice == "swap" and len(chars) > 3:
        i = random.randint(1, len(chars) - 2)
        chars[i], chars[i + 1] = chars[i + 1], chars[i]
    elif choice == "drop":
        i = random.randint(1, len(chars) - 2)
        chars.pop(i)
    elif choice == "dup":
        i = random.randint(1, len(chars) - 2)
        chars.insert(i, chars[i])

    return "".join(chars)


def alt_caps(text: str) -> str:
    out = []
    up = True

    for ch in text:
        if ch.isalpha():
            out.append(ch.upper() if up else ch.lower())
            up = not up
        else:
            out.append(ch)

    return "".join(out)


LEET_MAP = {
    "a": "4", "A": "4",
    "e": "3", "E": "3",
    "i": "1", "I": "1",
    "o": "0", "O": "0",
    "s": "5", "S": "5",
    "t": "7", "T": "7",
    "а": "4", "А": "4",
    "е": "3", "Е": "3",
    "о": "0", "О": "0",
    "с": "5", "С": "5",
    "т": "7", "Т": "7",
    "ы": "bl", "Ы": "BL",
}


def to_leet(text: str) -> str:
    out = []
    for ch in text:
        out.append(LEET_MAP.get(ch, ch))
    return "".join(out)


HELP_TEXT = (
    "🧩 Команды:\n"
    ".trol <текст|шаблон> [|мс]\n"
    ".troluse @юзер <текст|шаблон> |мс\n"
    ".trolerr <текст|шаблон>\n"
    ".trolrand <текст|шаблон>\n"
    ".trolupper / .trollower\n"
    ".trolcaps\n"
    ".trolleet\n"
    ".trolrandtpl\n"
    ".trolreal <текст>\n"
    ".trolreal2 <текст> <мин>-<макс>\n"
    ".spam <слово> [кол-во][задержка]\n"
    ".pause / .resume / .stop\n"
    ".trolstatus\n"
    ".ping\n"
    ".addshabl <имя> <текст>"
)


class TrolPlugin(BasePlugin):
    def on_plugin_load(self):
        self.add_on_send_message_hook()

    def on_send_message_hook(self, account: int, params: Any) -> HookStrategy:
        global SPAM_ACTIVE, SPAM_STARTED_AT
        global LAST_PING_MS

        msg = params.message if isinstance(params.message, str) else ""
        if not msg:
            return HookResult()

        if msg.strip() == ".ping":
            t0 = time.time()
            send_message({"peer": params.peer, "message": "🏓 Pong"})
            LAST_PING_MS = int((time.time() - t0) * 1000)
            send_message({"peer": params.peer, "message": f"🏓 Ping: {LAST_PING_MS} ms"})
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.strip() == ".trolstatus":
            ping_line = f"\n🏓 Ping: {LAST_PING_MS} ms" if LAST_PING_MS is not None else ""
            if not SPAM_ACTIVE:
                send_message({"peer": params.peer, "message": "🔴 Статус: неактивен" + ping_line})
                return HookResult(strategy=HookStrategy.CANCEL)

            state = "⏸ на паузе" if PAUSE_EVENT.is_set() else "🟢 активен"
            uptime = int(time.time() - SPAM_STARTED_AT)
            send_message({"peer": params.peer, "message": f"{state}\n⏱ Работает: {uptime} сек{ping_line}"})
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".addshabl "):
            raw = msg[len(".addshabl "):].strip()
            if not raw:
                send_message({"peer": params.peer, "message": "❌ Формат: .addshabl <имя> <текст>"})
                return HookResult(strategy=HookStrategy.CANCEL)

            parts = raw.split(maxsplit=1)
            name = parts[0].strip().lower()
            text = parts[1] if len(parts) > 1 else ""

            if not name or not text:
                send_message({"peer": params.peer, "message": "❌ Формат: .addshabl <имя> <текст>"})
                return HookResult(strategy=HookStrategy.CANCEL)

            if len(name) > 32:
                send_message({"peer": params.peer, "message": "❌ Имя слишком длинное (до 32 символов)"})
                return HookResult(strategy=HookStrategy.CANCEL)

            TEMPLATES[name] = text
            send_message({"peer": params.peer, "message": f"✅ Шаблон добавлен: {name}"})
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.strip() == ".stop":
            STOP_EVENT.set()
            PAUSE_EVENT.clear()
            SPAM_ACTIVE = False
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.strip() == ".pause":
            PAUSE_EVENT.set()
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.strip() == ".resume":
            PAUSE_EVENT.clear()
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.strip() == ".help":
            send_message({"peer": params.peer, "message": HELP_TEXT})
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".spam "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".spam "):]
            word, count, delay_ms, jitter_pct = parse_spam_args(raw, DEFAULT_DELAY_MS)
            run_on_queue(lambda: send_spam(params.peer, word, count, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trol "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trol "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = resolve_template_or_text(text)
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".troluse "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".troluse "):].strip()
            parts = raw.split(maxsplit=1)
            mention = parts[0]
            rest = parts[1] if len(parts) > 1 else ""
            text, delay_ms, jitter_pct = parse_params(rest)
            text = resolve_template_or_text(text)
            words = [f"{mention} {w}" for w in text.split()]
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolerr "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolerr "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = resolve_template_or_text(text)
            words = [distort_word(w) for w in text.split()]
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolrand "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolrand "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = resolve_template_or_text(text)
            words = text.split()
            random.shuffle(words)
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolupper "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolupper "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = resolve_template_or_text(text).upper()
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trollower "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trollower "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = resolve_template_or_text(text).lower()
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolcaps "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolcaps "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = alt_caps(resolve_template_or_text(text))
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolleet "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolleet "):]
            text, delay_ms, jitter_pct = parse_params(raw)
            text = to_leet(resolve_template_or_text(text))
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolrandtpl"):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolrandtpl"):].strip()
            text, delay_ms, jitter_pct = parse_params(raw)
            if TEMPLATES:
                text = random.choice(list(TEMPLATES.values()))
            words = text.split()
            run_on_queue(lambda: send_words(params.peer, words, delay_ms, jitter_pct))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolreal "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolreal "):]
            text = resolve_template_or_text(raw)
            words = text.split()
            run_on_queue(lambda: send_words_real(params.peer, words))
            return HookResult(strategy=HookStrategy.CANCEL)

        if msg.startswith(".trolreal2 "):
            STOP_EVENT.clear()
            SPAM_ACTIVE = True
            SPAM_STARTED_AT = time.time()
            raw = msg[len(".trolreal2 "):].strip()
            text_part, min_mul, max_mul = parse_real2_params(raw)
            text = resolve_template_or_text(text_part) if text_part else resolve_template_or_text(raw)
            words = text.split()
            run_on_queue(lambda: send_words_real2(params.peer, words, min_mul, max_mul))
            return HookResult(strategy=HookStrategy.CANCEL)

        return HookResult()
