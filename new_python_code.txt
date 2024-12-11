from flask import Flask, request
import requests

app = Flask(__name__)

TOKEN = "7585004355:AAHcNj2Vr77BbYDA4IEYFgyj44D-uNDTIow"

@app.route("/", methods=["GET"])
def home():
    return "Bot Çalışıyor!"

@app.route(f"/{TOKEN}", methods=["POST"])
def telegram_webhook():
    update = request.json

    if "message" in update and update["message"]["text"] == "/start":
        chat_id = update["message"]["chat"]["id"]
        message = "Merhaba! Oyunu başlatmak için butona tıklayın."
        button_url = "https://verdant-ganache-8978e8.netlify.app"  # Oyun URL'si

        # Web App butonu ekleme
        payload = {
            "chat_id": chat_id,
            "text": message,
            "reply_markup": {
                "keyboard": [[
                    {"text": "Oyunu Başlat", "web_app": {"url": button_url}}
                ]],
                "resize_keyboard": True
            }
        }

        requests.post(f"https://api.telegram.org/bot{TOKEN}/sendMessage", json=payload)

    return "ok"

if __name__ == "__main__":
    app.run(port=5000)
