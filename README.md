# ADNANX-PREMIUMimport logging
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes
from datetime import datetime

# Enable logging
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    level=logging.INFO
)
logger = logging.getLogger(__name__)

# Bot Token - اپنا token یہاں ڈالیں
BOT_TOKEN = "YOUR_BOT_TOKEN_HERE"

# User data (Demo purposes)
user_data = {
    "8993297363": {
        "username": "@adnan",
        "user_id": "8993297363",
        "membership": "🥉 Bronze",
        "balance": "$0.00",
        "total_spent": "$0.00",
        "referrals": 0,
        "referral_earning": "$0.00",
        "referral_link": "https://t.me/adnanx_premium_bot?start=ref_8993297363"
    }
}

# Products data
products = [
    {"name": "🔗 LINKEDIN CAREER 3M", "price": "$0.70", "stock": "78", "icon": "in"},
    {"name": "🔗 LINKEDIN CAREER 2M", "price": "$0.44", "stock": "6", "icon": "in"},
    {"name": "✂️ Capcut Pro 1 Month", "price": "$1.60", "stock": "0", "icon": "✂️"},
    {"name": "🎨 Canva Business Panel Paid", "price": "$2.50", "stock": "27", "icon": "🎨"},
    {"name": "🎬 LEONARDO AI VIDEO GEN", "price": "$0.70", "stock": "0", "icon": "🎬"},
    {"name": "🔮 Perplexity Enterprise Pro 1m", "price": "$6.00", "stock": "5", "icon": "🔮"},
    {"name": "💬 ChatGPT Plus Apple Pay 1M", "price": "$20.00", "stock": "3", "icon": "💬"},
    {"name": "🎬 Google Veo 3 Ultra Extension", "price": "$5.00", "stock": "7", "icon": "🎬"},
    {"name": "🎥 Netflix Premium 4K UHD", "price": "$15.00", "stock": "12", "icon": "🎥"},
    {"name": "▶️ Amazon Prime Video 6 Month", "price": "$1.50", "stock": "8", "icon": "▶️"},
    {"name": "📔 Notion Business 3 month", "price": "$1.50", "stock": "15", "icon": "📔"},
    {"name": "🎧 Headspace Premium 4 Month", "price": "$2.00", "stock": "12", "icon": "🎧"},
]

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Start command handler"""
    user_id = str(update.effective_user.id)
    user = update.effective_user
    
    if user_id not in user_data:
        user_data[user_id] = {
            "username": f"@{user.username}" if user.username else user.first_name,
            "user_id": user_id,
            "membership": "🥉 Bronze",
            "balance": "$0.00",
            "total_spent": "$0.00",
            "referrals": 0,
            "referral_earning": "$0.00",
            "referral_link": f"https://t.me/adnanx_premium_bot?start=ref_{user_id}"
        }
    
    welcome_text = f"""
╔══════════════════════════════════╗
║  🎯 ADNANX PREMIUM 🎯
║  Quality products at cheapest rates
╚══════════════════════════════════╝

👋 Welcome back, {user.first_name}!
Quality products at cheapest rates

{'─' * 36}
🏷️ Username: {user_data[user_id]['username']}
🆔 UserID: {user_data[user_id]['user_id']}
👑 Membership: {user_data[user_id]['membership']}
💰 Balance: {user_data[user_id]['balance']}
💎 Total Spent: {user_data[user_id]['total_spent']}
🤝 Referrals: {user_data[user_id]['referrals']}
💸 Referral Earning: {user_data[user_id]['referral_earning']}
🔗 Referral Link: {user_data[user_id]['referral_link']}
{'─' * 36}

Choose an option below to get started.
"""
    
    keyboard = [
        [InlineKeyboardButton("🛍️ SHOP", callback_data="shop")],
        [
            InlineKeyboardButton("💰 Wallet", callback_data="wallet"),
            InlineKeyboardButton("🎁 Freebies", callback_data="freebies"),
            InlineKeyboardButton("😊 Profile", callback_data="profile")
        ],
        [InlineKeyboardButton("🎯 Referral Store", callback_data="referral_store")],
        [
            InlineKeyboardButton("📞 Support", callback_data="support"),
            InlineKeyboardButton("✉️ Emails & Trials", callback_data="emails")
        ],
        [
            InlineKeyboardButton("🔧 Reseller API", callback_data="reseller"),
            InlineKeyboardButton("🗑️ Clear Chat", callback_data="clear_chat")
        ]
    ]
    
    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text(welcome_text, reply_markup=reply_markup, parse_mode='HTML')

async def shop(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Shop/Store handler"""
    query = update.callback_query
    await query.answer()
    
    shop_text = "🛍️ <b>SHOP - AVAILABLE PRODUCTS</b>\n\n"
    
    keyboard = []
    for i, product in enumerate(products):
        button_text = f"{product['name']} | {product['price']} | 📦 {product['stock']}"
        keyboard.append([InlineKeyboardButton(button_text, callback_data=f"product_{i}")])
    
    keyboard.append([InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")])
    
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(shop_text, reply_markup=reply_markup, parse_mode='HTML')

async def profile(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Profile handler"""
    query = update.callback_query
    await query.answer()
    user_id = str(query.from_user.id)
    user_info = user_data.get(user_id, {})
    
    profile_text = f"""
<b>👤 YOUR PROFILE</b>

{'─' * 36}
🏷️ Username: {user_info.get('username', 'N/A')}
🆔 UserID: {user_info.get('user_id', 'N/A')}
👑 Membership: {user_info.get('membership', 'N/A')}
💰 Balance: {user_info.get('balance', '$0.00')}
💎 Total Spent: {user_info.get('total_spent', '$0.00')}
🤝 Referrals: {user_info.get('referrals', 0)}
💸 Referral Earning: {user_info.get('referral_earning', '$0.00')}
{'─' * 36}
"""
    
    keyboard = [[InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(profile_text, reply_markup=reply_markup, parse_mode='HTML')

async def wallet(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Wallet handler"""
    query = update.callback_query
    await query.answer()
    
    wallet_text = """
<b>💰 WALLET</b>

<b>Current Balance: $0.00</b>

Please choose an option:
"""
    
    keyboard = [
        [InlineKeyboardButton("➕ Add Money", callback_data="add_money")],
        [InlineKeyboardButton("➖ Withdraw", callback_data="withdraw")],
        [InlineKeyboardButton("📊 Transaction History", callback_data="history")],
        [InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]
    ]
    
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(wallet_text, reply_markup=reply_markup, parse_mode='HTML')

async def freebies(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Freebies handler"""
    query = update.callback_query
    await query.answer()
    
    freebies_text = """
<b>🎁 FREEBIES</b>

No free products available at the moment.

Check back soon for amazing free offers!
"""
    
    keyboard = [[InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(freebies_text, reply_markup=reply_markup, parse_mode='HTML')

async def referral_store(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Referral Store handler"""
    query = update.callback_query
    await query.answer()
    
    referral_text = """
<b>🎯 REFERRAL STORE</b>

Earn money by referring your friends!

<b>How it works:</b>
1️⃣ Share your referral link
2️⃣ Your friends join using your link
3️⃣ Earn commission on every purchase they make!

Share your link and start earning today! 💸
"""
    
    keyboard = [
        [InlineKeyboardButton("📤 Share Referral Link", callback_data="share_ref")],
        [InlineKeyboardButton("💰 Referral Earnings", callback_data="ref_earnings")],
        [InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]
    ]
    
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(referral_text, reply_markup=reply_markup, parse_mode='HTML')

async def support(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Support handler"""
    query = update.callback_query
    await query.answer()
    
    support_text = """
<b>📞 SUPPORT</b>

Need help? We're here for you!

<b>Contact Us:</b>
📧 Email: support@adnanxpremium.com
💬 Telegram: @AdnanXSupport
⏰ Response Time: Within 24 hours

We're available 24/7 to help you! ✅
"""
    
    keyboard = [[InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(support_text, reply_markup=reply_markup, parse_mode='HTML')

async def emails(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Emails & Trials handler"""
    query = update.callback_query
    await query.answer()
    
    emails_text = """
<b>✉️ EMAILS & TRIALS</b>

Get premium accounts and trials at discounted prices!

Coming soon with special offers...
"""
    
    keyboard = [[InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(emails_text, reply_markup=reply_markup, parse_mode='HTML')

async def reseller(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Reseller API handler"""
    query = update.callback_query
    await query.answer()
    
    reseller_text = """
<b>🔧 RESELLER API</b>

Want to become a reseller? 

Our API allows you to:
✅ Integrate with your business
✅ Automate orders
✅ Get wholesale prices
✅ Dedicated support

📧 Email: api@adnanxpremium.com
"""
    
    keyboard = [[InlineKeyboardButton("⬅️ Back", callback_data="back_to_menu")]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await query.edit_message_text(reseller_text, reply_markup=reply_markup, parse_mode='HTML')

async def clear_chat(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Clear chat handler"""
    query = update.callback_query
    await query.answer("Chat history cleared! ✅", show_alert=True)
    await query.delete_message()

async def back_to_menu(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Back to main menu"""
    await start(update, context)

async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Handle all button callbacks"""
    query = update.callback_query
    await query.answer()
    
    if query.data == "shop":
        await shop(update, context)
    elif query.data == "profile":
        await profile(update, context)
    elif query.data == "wallet":
        await wallet(update, context)
    elif query.data == "freebies":
        await freebies(update, context)
    elif query.data == "referral_store":
        await referral_store(update, context)
    elif query.data == "support":
        await support(update, context)
    elif query.data == "emails":
        await emails(update, context)
    elif query.data == "reseller":
        await reseller(update, context)
    elif query.data == "clear_chat":
        await clear_chat(update, context)
    elif query.data == "back_to_menu":
        await back_to_menu(update, context)
    elif query.data.startswith("product_"):
        product_idx = int(query.data.split("_")[1])
        product = products[product_idx]
        product_text = f"""
<b>{product['name']}</b>

<b>Price:</b> {product['price']}
<b>In Stock:</b> {product['stock']}

{'✅ Available' if int(product['stock']) > 0 else '❌ Out of Stock'}

Want to order? Contact support!
"""
        keyboard = [[InlineKeyboardButton("⬅️ Back to Shop", callback_data="shop")]]
        reply_markup = InlineKeyboardMarkup(keyboard)
        await query.edit_message_text(product_text, reply_markup=reply_markup, parse_mode='HTML')

def main() -> None:
    """Start the bot."""
    application = Application.builder().token(BOT_TOKEN).build()
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CallbackQueryHandler(button_handler))
    application.run_polling()

if __name__ == '__main__':
    main()
