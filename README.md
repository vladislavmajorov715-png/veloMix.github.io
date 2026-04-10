[main.py](https://github.com/user-attachments/files/26624273/main.py)
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
import random

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
)

def random_point(lat, lon, radius_km):
    r = radius_km / 111.32
    u = random.random()
    v = random.random()
    w = r * u ** 0.5
    t = 2 * 3.14159 * v
    x = w * (t.cos())  # поправка на долготу
    y = w * (t.sin())
    return lat + y, lon + x

@app.get("/python-app/")
async def get_point(lat: float, lon: float, radius: int):
    r_lat, r_lon = random_point(lat, lon, radius)

    # Для простоты — статичная погода
    weather = "Сейчас +18°C, ясно"

    return {
        "lat": r_lat,
        "lon": r_lon,
        "weather": weather,
        "radius": radius
    }
