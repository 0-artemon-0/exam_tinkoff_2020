# exam_tinkoff_2020
# TicTacToe project for ml exams to tinkoff generation.


import random
from random import randint


def check_winner(field_):
    for y in range(n):
        for x in range(n):
            # check left
            if x + k - 1 < n:
                cnt_x = 0
                cnt_o = 0
                for q in range(k):
                    if field_[y][x + q] == 'x':
                        cnt_x += 1
                    elif field_[y][x + q] == 'o':
                        cnt_o += 1
                    else:
                        break
                if cnt_x == k:
                    return 'x'
                elif cnt_o == k:
                    return 'o'

            # check down
            if y + k - 1 < n:
                cnt_x = 0
                cnt_o = 0
                for q in range(k):
                    if field_[y + q][x] == 'x':
                        cnt_x += 1
                    elif field_[y + q][x] == 'o':
                        cnt_o += 1
                    else:
                        break
                if cnt_x == k:
                    return 'x'
                elif cnt_o == k:
                    return 'o'

            # check left - down
            if y + k - 1 < n and x + k - 1 < n:
                cnt_x = 0
                cnt_o = 0
                for q in range(k):
                    if field_[y + q][x + q] == 'x':
                        cnt_x += 1
                    elif field_[y + q][x + q] == 'o':
                        cnt_o += 1
                    else:
                        break
                if cnt_x == k:
                    return 'x'
                elif cnt_o == k:
                    return 'o'

            # check right - down
            if y + k - 1 < n and x >= k - 1:
                cnt_x = 0
                cnt_o = 0
                for q in range(k):
                    if field_[y + q][x - q] == 'x':
                        cnt_x += 1
                    elif field_[y + q][x - q] == 'o':
                        cnt_o += 1
                    else:
                        break
                if cnt_x == k:
                    return 'x'
                elif cnt_o == k:
                    return 'o'

    end = 1
    for y in range(n):
        for x in range(n):
            if field_[y][x] == '.':
                end = 0

    if end:
        return '!'

    return '-'


def x_turn():
    while 1:
        print("'x' turn. Enter in one string x and y coordinate of field to place 'x' there")
        x, y = [int(i) for i in input().split()]
        x -= 1
        y -= 1
        if field[y][x] == 'x':
            print("This field is already has 'x', choose another field")
        elif field[y][x] == 'o':
            print("This field is already has 'o', choose another field")
        else:
            field[y][x] = 'x'
            print("Ok, your turn has accepted")
            break

def o_turn():
    while 1:
        print("'o' turn. Enter in one string x and y coordinate of field to place 'o' there")
        x, y = [int(i) for i in input().split()]
        x -= 1
        y -= 1
        if field[y][x] == 'x':
            print("This field is already has 'x', choose another field")
        elif field[y][x] == 'o':
            print("This field is already has 'o', choose another field")
        else:
            field[y][x] = 'o'
            print("Ok, your turn has accepted")
            break

def get_wins_x(b):
    for i in range(len(b)):
        r = random.randint(1, i + 1) - 1
        b[i], b[r] = b[r], b[i]

    #print('b = ', b)

    for i in range(len(b)):
        if i % 2 == 1:
            field[b[i][0]][b[i][1]] = 'x'
        else:
            field[b[i][0]][b[i][1]] = 'o'

        tmp = check_winner(field)

        if tmp == 'x':
            return 1
        elif tmp == 'o':
            return -1
        elif tmp == '!':
            return 0

def x_AI_turn():
    a = []
    for i in range(n):
        for j in range(n):
            if field[i][j] == '.':
                a.append([i, j])

    mx = -10000
    mx_turn = [0, -1]
    for i in range(len(a)):
        start = a[i]
        cell = []
        field[start[0]][start[1]] = 'x'
        for j in range(len(a)):
            if j != i:
                cell.append(a[j])

        wins = 0
        for j in range(min(400 // (len(cell) + 1), 40) * diff):
            for q in range(len(cell)):
                field[cell[q][0]][cell[q][1]] = '.'
            wins += get_wins_x(cell)

        field[start[0]][start[1]] = '.'
        if wins > mx:
            mx = wins
            mx_turn = start

    #print(mx, min(10 * len(cell), 500), ' for AI')
    for q in range(len(cell)):
        field[cell[q][0]][cell[q][1]] = '.'
    field[mx_turn[0]][mx_turn[1]] = 'x'
            
def get_wins_o(b):
    for i in range(len(b)):
        r = random.randint(1, i + 1) - 1
        b[i], b[r] = b[r], b[i]

    #print('b = ', b)

    for i in range(len(b)):
        if i % 2 == 0:
            field[b[i][0]][b[i][1]] = 'x'
        else:
            field[b[i][0]][b[i][1]] = 'o'

        tmp = check_winner(field)

        if tmp == 'x':
            return -1
        elif tmp == 'o':
            return 1
        elif tmp == '!':
            return 0

def o_AI_turn():
    a = []
    for i in range(n):
        for j in range(n):
            if field[i][j] == '.':
                a.append([i, j])

    mx = -10000
    mx_turn = [0, -1]
    for i in range(len(a)):
        start = a[i]
        cell = []
        field[start[0]][start[1]] = 'o'
        for j in range(len(a)):
            if j != i:
                cell.append(a[j])

        wins = 0
        for j in range(min(400 // (len(cell) + 1), 40) * diff):
            for q in range(len(cell)):
                field[cell[q][0]][cell[q][1]] = '.'
            wins += get_wins_o(cell)

        field[start[0]][start[1]] = '.'
        if wins > mx:
            mx = wins
            mx_turn = start

    #print(mx, min(10 * len(cell), 500), ' for AI')
    for q in range(len(cell)):
        field[cell[q][0]][cell[q][1]] = '.'
    field[mx_turn[0]][mx_turn[1]] = 'o'


def print_field():
    for i in field:
        for j in i:
            print(j, end = ' ')
        print()
    print()


print("Welcome to tic-tac-toe!")
n = 0
while 1:
    print("Enter the lenght of field's side (from 3 to 7)")
    n = input()
    if n == '3':
        n = 3
        break
    elif n == '4':
        n = 4
        break
    elif n == '5':
        n = 5
        break
    elif n == '6':
        n = 6
        break
    elif n =='7':
        n = 7
        break
    else:
        print("Wrong input!!!")
        
field = [['.'] * n for i in range(n)]
k = 0
while 1:
    print("Now enter how much elements in row leads to win (from 3 to n)")
    k = input()
    if k == '3':
        k = 3
        break
    elif k == '4' and n >= 4:
        k = 4
        break
    elif k == '5' and n >= 5:
        k = 5
        break
    elif k == '6' and n >= 6:
        k = 6
        break
    elif k == '7' and n >= 7:
        k = 7
        break
    else:
        print("Wrong input!!!")

print("Good! Now you can choose game mode")

while 1:
    print("Print 'pvp' if you want to play versus human")
    print("Print 'pve' if you want to play versus AI")
    print("Print 'eve' if you want to see two AI duel")
    mode = input()  # pvp, pve or eve (p - player, v - versus, e - enviroment) - classic names of modes
    if mode != 'pvp' and mode != 'pve' and mode != 'eve':
        print("Wrong input!!!")
    else:
        break

diff = 1
        
if mode == 'pve' or mode == 'eve':
    print("Now choose the difficulty. How good will AI play")
    while 1:
        print("Print 'easy', 'medium', 'hard', 'extreme' or 'custom'")
        tmp = input()
        if tmp == 'easy':
            diff = 1
            break
        elif tmp == 'medium':
            diff = 5
            break
        elif tmp == 'hard':
            diff = 10
            break
        elif tmp == 'extreme':
            diff = 20
            break
        elif tmp == 'custom':
            print("Print the number of seconds that the average AI will spend on a move")
            # число секунд которое ИИ будет в среднем тратить на ход
            diff = int(input())
        else:
            print("Wrong input!!!")

print_field()

if mode == 'pvp':
    i = 0
    while check_winner(field) == '-':
        if i % 2 == 0:
            x_turn()
        else:
            o_turn()
        
        print_field()
        i += 1
    if check_winner(field) == 'x':
        print("!!! 'x' wins !!!")
    elif check_winner(field) == 'o':
        print("!!! 'o' wins !!!")
    else:
        print("It's tie. Maybe another match?")
        
elif mode == 'pve':
    print("You will play for 'x' or for 'o'?")
    turn = input()

    print_field()
    if turn == 'x':
        i = 0
        while check_winner(field) == '-':
            if i % 2 == 0:
                x_turn()
            else:
                o_AI_turn()

            print_field()
            i += 1

        if check_winner(field) == 'x':
            print("!!! 'x' wins !!! WOW you defeated AI !!! Congratulations !!!")
        elif check_winner(field) == 'o':
            print(" :( AI wins. Good luck next time")
        else:
            print("It's tie. Maybe another match?")
    elif turn == 'o':
        i = 0
        while check_winner(field) == '-':
            if i % 2 == 1:
                o_turn()
            else:
                x_AI_turn()

            print_field()
            i += 1

        if check_winner(field) == 'o':
            print("!!! 'o' wins !!! WOW you defeated AI !!! Congratulations !!!")
        elif check_winner(field) == 'o':
            print(" :( AI wins. Good luck next time")
        else:
            print("It's tie. Maybe another match?")
elif mode == 'eve':
    i = 0
    while check_winner(field) == '-':
        if i % 2 == 0:
            x_AI_turn()
        else:
            o_AI_turn()

        print_field()
        i += 1
    if check_winner(field) == 'x':
        print("!!! 'x' wins !!!")
    elif check_winner(field) == 'o':
        print("!!! 'o' wins !!!")
    else:
        print("It's tie.")
