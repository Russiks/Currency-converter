import telebot  # Импортируем библиотеку pyTelegramBotApi для работы с телеграм-ботом
import traceback  # Импортируем библиотеку traceback для вывода трассировки стэка
from telebot import types  # Для работы с кнопками импортируем модуль types из telebot

from extensions import ApiException, Converter
from config import exchanges, TOKEN  # Импортируем всё из файла config.py

bot = telebot.TeleBot(TOKEN)  # Инициализируем объект класса TeleBot через библиотеку pyTelegramBotApi и передаем токен


# в его конструктор


@bot.message_handler(commands=['start'])  # Обучаем бота отвечать на сообщения (команды: '/start')
def start(message: telebot.types.Message):
    start_button = types.ReplyKeyboardMarkup(resize_keyboard=True)
    start_button.add(types.KeyboardButton(text='/help'))
    bot.send_message(message.chat.id, f"Добро пожаловать, {message.chat.first_name}!\n"
                                      # Обучаем бота обращаться по 'username' к пользователю 
                                      f"Я Телеграм-бот MoneyBoty.\n"
                                      f"Я рад буду помочь тебе в конвертирование валют.\n"
                                      f"До начала работы, узнай, что я умею, по команде /help", reply_markup=start_button)


@bot.message_handler(commands=['help'])  # Обучаем бота отвечать на сообщения (команды: '/help')
def helpy(message: telebot.types.Message):
    start_button = types.ReplyKeyboardMarkup(resize_keyboard=True)
    start_button.add(types.KeyboardButton(text='/values'))
    start_button.add(types.KeyboardButton(text='/example'))
    bot.send_message(message.chat.id, f"{message.chat.first_name}, список доступных валют ты можешь узнать по команде"
                                      f" /values.\n"
                                      f"А пример ввода по команде /example\n"
                                      f"Чтобы начать работу введи:\n "
                                      f"<название валюты, цену которой ты хочешь узнать>\n"
                                      f"<название валюты, в которой надо узнать цену первой валюты>\n"
                                      f"<количество первой валюты>", reply_markup=start_button)
    # Метод 'send_message' просто отвечает на сообщение


@bot.message_handler(commands=['example'])  # Обучаем бота отвечать на сообщения (команды: '/example')
def helpy(message: telebot.types.Message):
    start_button = types.ReplyKeyboardMarkup(resize_keyboard=True)
    start_button.add(types.KeyboardButton(text='/values'))
    bot.send_message(message.chat.id, f"{message.chat.first_name}, посмотри на пример ввода валют:\n"
                                      f"-> Доллар рубль 1\n"
                                      f"<Где доллар, валюта, цену которой надо узнать>\n"
                                      f"<Где рубль, валюта, в который ты хочешь узнать цену>\n"
                                      f"<А 1 (1,5 или 55.9 и т.д) количество валюты, цену которой ты хочешь узнать>\n"
                                      f"Вводить так же можно вещественные числа, например: 1,5 или 2.7 и т.д;)", reply_markup=start_button)
    # Метод 'send_message' просто отвечает на сообщение


@bot.message_handler(commands=['values'])  # Обучаем бота отвечать на сообщения (команды: '/values')
def values(message: telebot.types.Message):
    start_button = types.ReplyKeyboardMarkup(one_time_keyboard=True, resize_keyboard=True)
    start_button.add(types.KeyboardButton(text='/example'))
    text = f"{message.chat.first_name}, для конвертации тебе доступны следующие валюты:"
    for i in exchanges.keys():  # Проходим в списке словаря по ключам
        text = '\n'.join((text, i))  # Записываем с новой строки каждый ключ
    bot.reply_to(message, text, reply_markup=start_button)
    # Метод 'reply_to' прикрепляет сообщение на которое отвечает


@bot.message_handler(content_types=['text'])  # Обучаем бота отвечать на текстовые сообщения
def converter(message: telebot.types.Message):
    value = message.text.split()  # Через метод .split() разделяем строку через пробел на список строк и возвращаем
    try:
        if len(value) != 3:  # Через функцию len() проверяем возвращаем количество введенных параметров (value) и если
            # это количество больше или меньше 3, то вызываем исключение и выводим сообщение:
            raise ApiException('Неверное количество параметров.\n'
                               'Попробуй снова!')

        answer = Converter.get_price(*value)  # В переменную get_price() добавляем введенные параметры
    except ApiException as e:  # для отлова исключений описанных в классе Converter через класс ApiException.
        # Вызываем исключение и выводим сообщение с соответствующей ошибкой.
        bot.reply_to(message, f"Ошибка в команде:\n{e}")
    except Exception as e:
        traceback.print_tb(e.__traceback__)  # Через traceback выводим сообщение о неизвестной ошибке,
        # которая не была указана в классе исключений.
        bot.reply_to(message, f"Неизвестная ошибка:\n{e}")
    else:
        bot.reply_to(message, answer)  # Соответствующее сообщение об ошибке в сообщение бота для пользователя


bot.polling()  # Команда, без которой программа отработает только один раз и завершиться.
