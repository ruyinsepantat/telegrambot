# telegrambot
برای کسب و کار کوچک خودم که در ان مصرف گرایی ترویج میشود
from fastapi import FastAPI
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext
import os

app = FastAPI()

# توکن بات تلگرام خود را وارد کنید
TELEGRAM_API_TOKEN = '7447332615:AAEksjeLa2hEXIDU2SRm8Ix1O1oacDztUXw'

# آیدی کانال اصلی
CHANNEL_ID = '@daushtarian'

# تابع بررسی عضویت
def check_membership(update: Update, context: CallbackContext):
    user_id = update.message.from_user.id
    chat_member = context.bot.get_chat_member(CHANNEL_ID, user_id)
    
    # بررسی وضعیت عضویت
    if chat_member.status in ['member', 'administrator', 'creator']:
        update.message.reply_text("خوش آمدید! شما به کانال اصلی عضو هستید.")
        # ادامه عملیات بات
    else:
        update.message.reply_text("لطفاً به کانال اصلی بپیوندید: @daushtarian.")

# تنظیمات تلگرام
def start(update: Update, context: CallbackContext):
    update.message.reply_text("سلام! برای ادامه، لطفاً به کانال اصلی بپیوندید.")
    check_membership(update, context)

# راه‌اندازی بات
updater = Updater(TELEGRAM_API_TOKEN, use_context=True)
dp = updater.dispatcher

# افزودن دستور start
dp.add_handler(CommandHandler("start", start))

@app.get("/")
def read_root():
    return {"message": "بات تلگرام در حال اجراست!"}

if __name__ == "__main__":
    updater.start_polling()
