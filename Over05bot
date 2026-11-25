import requests
import time
from datetime import datetime, timezone

TELEGRAM_TOKEN = "9273636150:AAG-WbSYB80zWzEpw9iD1coUAPyi7HSAQZQ"
CHAT_ID = "5803856763"
ODDS_API_KEY = "38e7754bcff7f4082b8a54033b1c0cea"

def manda(testo):
    url = f"https://api.telegram.org/bot{TELEGRAM_TOKEN}/sendMessage"
    requests.post(url, json={"chat_id": CHAT_ID, "text": testo})

manda("✅ Bot su RENDER acceso! Sempre attivo 24/24")

SPORTS = ["soccer_italy_serie_a", "soccer_epl", "soccer_spain_la_liga", "soccer_germany_bundesliga", "soccer_france_ligue_1", "soccer_uefa_champs_league", "soccer_uefa_europa_league", "soccer_uefa_europa_conference_league"]
gia = set()

while True:
    try:
        ora = datetime.now(timezone.utc)
        for sport in SPORTS:
            url = f"https://api.the-odds-api.com/v4/sports/{sport}/odds/?apiKey={ODDS_API_KEY}&regions=eu&markets=totals,spreads&oddsFormat=decimal"
            data = requests.get(url).json()
            if not isinstance(data, list):
                continue
            for match in data:
                try:
                    inizio = datetime.fromisoformat(match.get("commence_time","").replace("Z", "+00:00"))
                except:
                    continue
                minuti = (ora - inizio).total_seconds() / 60
                if minuti < 0 or minuti > 95:
                    continue
                nome = f"{match.get('home_team','')} vs {match.get('away_team','')}"
                for book in match.get("bookmakers", []):
                    for m in book.get("markets", []):
                        if m.get("key") == "totals":
                            for o in m.get("outcomes", []):
                                if o.get("name") == "Over" and o.get("point") == 0.5:
                                    q = o.get("price", 0)
                                    chiave = nome + "_over05"
                                    if 0.60 <= q <= 0.85 and 20 <= minuti <= 35 and chiave not in gia:
                                        manda(f"🔥 LIVE {int(minuti)}' - OVER 0.5 1T\n\n⚽ {nome}\n💰 @{q}")
                                        gia.add(chiave)
                                if o.get("name") == "Over" and o.get("point") == 1.5:
                                    q = o.get("price", 0)
                                    chiave = nome + "_forever"
                                    if 1.60 <= q <= 2.10 and 60 <= minuti <= 80 and chiave not in gia:
                                        manda(f"♾️ LIVE {int(minuti)}' - FOREVER 1 BALL\n\n⚽ {nome}\n💰 @{q}")
                                        gia.add(chiave)
                        if m.get("key") == "spreads":
                            for o in m.get("outcomes", []):
                                if o.get("point") == -0.25:
                                    q = o.get("price", 0)
                                    chiave = nome + "_magia_" + str(o.get("name"))
                                    if 1.80 <= q <= 2.20 and 55 <= minuti <= 75 and chiave not in gia:
                                        manda(f"✨ LIVE {int(minuti)}' - MAGIA -0.25\n\n⚽ {nome}\n👉 {o.get('name')}\n💰 @{q}")
                                        gia.add(chiave)
        time.sleep(300)
    except Exception as e:
        print("Errore:", e)
        time.sleep(60)
