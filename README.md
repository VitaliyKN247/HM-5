# HM-5

1. '''
Напишите программу, удаляющую из текста все слова, содержащие ""абв"".
'''
str = 'азбука абв рак вот так абвгдейка телефон просто абвоа'

words = str.split(' ')
fragment = 'абв'
new_str = []

for i in words:
    if fragment not in i:
        new_str.append(i)

print(*new_str)


2.'''
Создайте программу для игры с конфетами человек против человека.

Условие задачи: На столе лежит 2021 конфета. Играют два игрока делая ход друг после друга. 
Первый ход определяется жеребьёвкой. За один ход можно забрать не более чем 28 конфет. 
Все конфеты оппонента достаются сделавшему последний ход. 
Сколько конфет нужно взять первому игроку, чтобы забрать все конфеты у своего конкурента?
a) Добавьте игру против бота
b) Подумайте как наделить бота ""интеллектом""
'''
from random import randint

candy = 102

players_1 = int(input('Введите цифру от 1 до 2: '))
players_2 = int(input('Введите цифру от 1 до 2: '))

rnd_number = randint(1,2)
print(rnd_number)

if players_1 == rnd_number:
    print('Начинает первый игрок!')
    leading_1 = players_1
    leading_2 = players_2
else:
    print('Начинает второй игрок!')
    leading_1 = players_2
    leading_2 = players_1

leading_1 = 0
leading_2 = 0
players_1 = 0
players_2 = 0
while candy >= 1:
    leading_1 = int(input('Ходит первый игрок. Берите не больше 28  конфет: '))
    if leading_1 >= 29 :
        print('Не жульничаем!')
    else:
        candy -= leading_1
        players_1 += leading_1
        leading_1 = 0
    print('Остаток конфет: ', candy)
    leading_2 = int(input('Ходит второй игрок. Берите не больше 28  конфет: '))
    if leading_2 >= 29:
        print('Не жульничаем!')
    else:
        candy -= leading_2
        players_2 += leading_2
        leading_2 = 0
    print('Остаток конфет: ', candy)

print(players_1, ' ', players_2)

if players_1 > players_2:
    print('Победа первого игрока! ')
else:
    print('Победа второго игрока! ')

print('Конец Игры!')


3.'''
Создайте программу для игры в ""Крестики-нолики"".
'''
maps = [1,2,3,
        4,5,6,
        7,8,9]

victories = [[0,1,2],
             [3,4,5],
             [6,7,8],
             [0,3,6],
             [1,4,7],
             [2,5,8],
             [0,4,8],
             [2,4,6]]

def print_maps():
    print(maps[0], end = ' ')
    print(maps[1], end = ' ')
    print(maps[2])
    
    print(maps[3], end = ' ')
    print(maps[4], end = ' ')
    print(maps[5])

    print(maps[6], end = ' ')
    print(maps[7], end = ' ')
    print(maps[8])

def step_maps(step,symbol):
    ind = maps.index(step)
    maps[ind] = symbol

def get_result():
    win = ''
    for i in victories:
        if maps[i[0]] == 'X' and maps [i[1]] == 'X' and maps [i[2]] == 'X':
            win = 'X'
        if maps[i[0]] == '0' and maps [i[1]] == '0' and maps [i[2]] == '0':
            win = '0'
    return win

game_over = False
player1 = True

while game_over == False:
    print_maps()
    if player1 == True:
        symbol = 'X'
        step = int(input('Ход первого игрока: '))
    else:
        symbol = '0'
        step = int(input('Ход второго игрока: '))

    step_maps(step,symbol)
    win = get_result()
    if win != '':
        game_over = True
    else:
        game_over = False
    
    player1 = not(player1)

print('Победитель', win)


4.'''
Реализуйте RLE алгоритм: реализуйте модуль сжатия и восстановления данных.
'''

with open('file_encode.txt', 'w') as data:
    data.write('WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW')

with open('file_encode.txt', 'r') as data:
    string = data.readline()

def rle_encode(decoded_string):
    encoded_string = ''
    count = 1
    char = decoded_string[0]
    for i in range(1, len(decoded_string)):
        if decoded_string[i] == char:
            count += 1
        else:
            encoded_string = encoded_string + str(count) + char
            char = decoded_string[i]
            count = 1
            encoded_string = encoded_string + str(count) + char
    return encoded_string


def rle_decode(encoded_string):
    decoded_string = ''
    char_amount = ''
    for i in range(len(encoded_string)):
        if encoded_string[i].isdigit():
            char_amount += encoded_string[i]
        else:
            decoded_string += encoded_string[i] * int(char_amount)
        char_amount = ''
    print(decoded_string)

    return decoded_string


with open('file_encode.txt', 'r') as file:
    decoded_string = file.read()

with open('file_decode.txt', 'w') as file:
    encoded_string = rle_encode(decoded_string)
    file.write(encoded_string)

print('Decoded string: \t' + decoded_string)
print('Encoded string: \t' + rle_encode(decoded_string))
print(f'Compress ratio: \t{round(len(decoded_string) / len(encoded_string), 1)}')
