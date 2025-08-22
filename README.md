import time
import requests
from telegram import Bot

# --- Configuración ---
BOT_TOKEN = "8158214796:AAFDbBKfNhTLhv_6hSSc6e-cu1Mu4H4PdO0"  # tu token del bot
CHAT_ID = "6464012938"  # tu chat ID

bot = Bot(token=BOT_TOKEN)

def get_price():
    """Obtiene el precio actual de LINK/USDT desde Binance"""
    url = "https://api.binance.com/api/v3/ticker/price?symbol=LINKUSDT"
    data = requests.get(url).json()
    return float(data["price"])

def check_strategy():
    """Calcula entrada y salida dinámicas y envía mensaje a Telegram"""
    price = get_price()

    # Estrategia simple (puedes ajustarla)
    entrada = price * 0.98   # 2% por debajo del precio actual
    salida  = price * 1.05   # 5% por encima del precio actual

    message = (
        f"📊 Estrategia Spot LINK/USDT\n"
        f"💰 Precio actual: {price:.2f} USDT\n\n"
        f"🟢 Entrada sugerida: {entrada:.2f} USDT\n"
        f"🔴 Salida sugerida: {salida:.2f} USDT"
    )

    bot.send_message(chat_id=CHAT_ID, text=message)

if __name__ == "__main__":
    while True:
        check_strategy()
        time.sleep(3600)  # Espera 1 hora antes de volver a enviar
