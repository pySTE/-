# -import telebot

bot = telebot.TeleBot("Your token")


@bot.message_handler(commands=['start'])
def start_message(message):
    bot.send_message(message.chat.id, "<b>Привет! Я могу помочь тебе решить квадратные уравнения.\nФормат ввода коэффицентa: без запятых через один пробел\nПример: \n1 2 3 </b>", parse_mode='html')

@bot.message_handler(func=lambda message: True)
def solve_equation(message):
    try:
        a, b, c = map(float, message.text.split())
    except ValueError:
        bot.send_message(message.chat.id, "<b>Неверный формат уравнения.</b>", parse_mode='html')
        return

    d = b ** 2 - 4 * a * c
    if d < 0:
        bot.send_message(message.chat.id, "<b>Уравнение не имеет корней.</b>", parse_mode='html')
    elif d == 0:
        x = -b / (2 * a)
        bot.send_message(message.chat.id, f"Корень уравнения: <b>{x}</b>", parse_mode='html')
    else:
        x1 = (-b + d ** 0.5) / (2 * a)
        x2 = (-b - d ** 0.5) / (2 * a)
        bot.send_message(message.chat.id, f"Корни уравнения: <b>{x1}</b>, <b>{x2}</b>", parse_mode='html')


bot.polling()
