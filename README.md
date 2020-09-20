# Chistyakov Artem. Tic-Tac-Toe project.
# exam_tinkoff_2020
# TicTacToe project for ml exams to tinkoff generation.
import random
from random import randint


def check_winner(): # this function checks winner.
    
    # It returns 'o' if the winner is 'o'
    # 'x' if the winner is 'x', '!' if it is tie (no winner and all field are used)
    # and '-' if the game is not finished
    
    # We iterate over start point of k-row and see 4 directions
    # Then we check all points from start in chosed direct
    for y in range(n):
        for x in range(n):
            # check left
            if x + k - 1 < n:
                cnt_x = 0
                cnt_o = 0
                for q in range(k):
                    if field[y][x + q] == 'x':
                        cnt_x += 1
                    elif field[y][x + q] == 'o':
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
                    if field[y + q][x] == 'x':
                        cnt_x += 1
                    elif field[y + q][x] == 'o':
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
                    if field[y + q][x + q] == 'x':
                        cnt_x += 1
                    elif field[y + q][x + q] == 'o':
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
                    if field[y + q][x - q] == 'x':
                        cnt_x += 1
                    elif field[y + q][x - q] == 'o':
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
            if field[y][x] == '.':
                end = 0

    if end:
        return '!'

    return '-'


def x_turn(): # This function used to input human's turn which is playing for 'x'
    
    # This function inputs turn and changes field if this turn is correct
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

def o_turn(): # This function used to input human's turn which is playing for 'o'
    
    # This function inputs turn and changes field if this turn is correct
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

def get_win_x(b): # This function returns who wins if both sides will take random turns after a given move 
    
    # This is random shuffle of given sequence using Fisher-Yates shuffle
    # You can read about this alho here: https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle
    for i in range(len(b)):
        r = random.randint(1, i + 1) - 1
        b[i], b[r] = b[r], b[i]

    # Now we make moves one by one in a given random sequence.

    for i in range(len(b)):
        if i % 2 == 1:
            field[b[i][0]][b[i][1]] = 'x'
        else:
            field[b[i][0]][b[i][1]] = 'o'

        # We also check winner after each turn
        tmp = check_winner()
    
        # We increase our bounty by 1 if AI wins game
        # We decrease our bounty by 1 if AI loses
        # If it is tie, we dont change bounty 
        if tmp == 'x':
            return 1
        elif tmp == 'o':
            return -1
        elif tmp == '!':
            return 0

def x_AI_turn(): # this function is AI playing x. AI choses the best turn and changes the field
    # classic debuts
    tmp_field = [['.'] * 5 for i in range(5)]
    if field == tmp_field:
        field[2][2] = 'x'
        return
    
    tmp_field[2][2] = 'x'
    tmp_field[1][2] = 'o'
    if field == tmp_field:
        field[1][1] = 'x'
        return
    
    tmp_field[1][2] = '.'
    tmp_field[2][1] = 'o'
    if field == tmp_field:
        field[1][1] = 'x'
        return
    
    tmp_field[2][1] = '.'
    tmp_field[3][2] = 'o'
    if field == tmp_field:
        field[1][1] = 'x'
        return
    
    tmp_field[3][2] = '.'
    tmp_field[2][3] = 'o'
    if field == tmp_field:
        field[1][1] = 'x'
        return
    
    
    # we store in a all possible moves
    a = []
    for i in range(n):
        for j in range(n):
            if field[i][j] == '.':
                a.append([i, j])

    mx_bounty = -10000 * diff
    best_turn = [0, -1]
    
    # How does it works?
    # We iterate over first AI turn.
    # Then do all other turns randomly one by one MANY TIMES. AI choses turn with the best bounty 
    
    # We iterate over first AI turn
    for i in range(len(a)):
        start_turn = a[i]
        cell = []
        
        # change field like if AI will do this turn
        field[start_turn[0]][start_turn[1]] = 'x'
        
        # cell is all other turns
        for j in range(len(a)):
            if j != i:
                cell.append(a[j])

        bounty = 0
        for j in range(min(600 // (len(cell) + 1), 60) * diff): # We do many iterations to reduce the impact of randomness
                
            # now we change bounty for this iteration
            bounty += get_win_x(cell) 
            
            # we restore field
            for q in range(len(cell)):
                field[cell[q][0]][cell[q][1]] = '.'
        
        # change back field
        field[start_turn[0]][start_turn[1]] = '.'
        
        # if new_bounty (bounty) > mx_bounty -> this turn is the best now -> update the bounty and the turn
        if bounty > mx_bounty:
            mx_bounty = bounty
            best_turn = start_turn

    # now AI makes turn. Change the field
    field[best_turn[0]][best_turn[1]] = 'x'
            
def get_win_o(b):
    # This function is simular to "get_wins_x", with some changes.
    # All comments to this funstion is simular to comments to "get_wins_x". For your convenience i will copy the comments
    
    
    # This function returns who wins if both sides will take random turns after a given move 
    
    # This is random shuffle of given sequence using Fisher-Yates shuffle
    # You can read about this alho here: https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle
    for i in range(len(b)):
        r = random.randint(1, i + 1) - 1
        b[i], b[r] = b[r], b[i]
    
    # Now we make moves one by one in a given random sequence.
    
    for i in range(len(b)):
        if i % 2 == 0:
            field[b[i][0]][b[i][1]] = 'x'
        else:
            field[b[i][0]][b[i][1]] = 'o'
        
        # We also check winner after each turn
        tmp = check_winner()
        
        # We increase our bounty by 1 if AI wins game
        # We decrease our bounty by 1 if AI loses
        # If it is tie, we dont change bounty 
        if tmp == 'x':
            return -1
        elif tmp == 'o':
            return 1
        elif tmp == '!':
            return 0

def o_AI_turn():
    # This function is simular to "x_AI_turn", with some changes.
    # All comments to this funstion is simular to comments to "x_AI_turn". For your convenience i will copy the comments
    
    tmp_field = [['.'] * 5 for i in range(5)]
    tmp_field[2][2] = 'x'
    if field == tmp_field:
        field[1][1] = 'o'
        return
    
    # this function is AI playing o. AI choses the best turn and changes the field
    # we store in a all possible moves
    a = []
    for i in range(n):
        for j in range(n):
            if field[i][j] == '.':
                a.append([i, j])

    mx_bounty = -10000 * diff
    best_turn = [0, -1]
    
    # How does it works?
    # We iterate over first AI turn.
    # Then do all other turns randomly one by one MANY TIMES. AI choses turn with the best bounty 
    
    # We iterate over first AI turn
    for i in range(len(a)):
        start_turn = a[i]
        cell = []
        
        # change field like if AI will do this turn
        field[start_turn[0]][start_turn[1]] = 'o'
        
        # cell is all other turns
        for j in range(len(a)):
            if j != i:
                cell.append(a[j])

        bounty = 0
        for j in range(min(600 // (len(cell) + 1), 60) * diff): # We do many iterations to reduce the impact of randomness
                
            # now we change bounty for this iteration
            bounty += get_win_o(cell)
                        
            # we restore field
            for q in range(len(cell)):
                field[cell[q][0]][cell[q][1]] = '.'
        
        # change back field
        field[start_turn[0]][start_turn[1]] = '.'
        
        # if new_bounty (bounty) > mx_bounty -> this turn is the best now -> update the bounty and the turn
        if bounty > mx_bounty:
            mx_bounty = bounty
            best_turn = start_turn

    # now AI makes turn. Change the field
    field[best_turn[0]][best_turn[1]] = 'o'


def print_field(): # this function prints field (nothing special)
    for i in field:
        for j in i:
            print(j, end = ' ')
        print()
    print()


def isint(s):
    try:
        int(s)
        return True
    except ValueError:
        return False
    
# Here is user interface. Many comments is simular to prints
print("Welcome to tic-tac-toe!")
n = 0
while 1:
    print("Enter the lenght of field's side (from 3 to 6)")
    print("I recomend not to print 6 if your computer is as calculator as mine")
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
    else:
        print("Wrong input!!!")
        
field = [['.'] * n for i in range(n)]
k = 0
while 1:
    print("Now enter how much elements in row leads to win (from 3 to 6)")
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
    else:
        print("Wrong input!!!")

print("Good! Now you can choose game mode")

while 1:
    print()
    print("Print 'pvp' if you want to play versus human")
    print("Print 'pve' if you want to play versus AI")
    print("Print 'eve' if you want to see two AI duel")
    print()
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
        print()
        print("easy AI spends 5 second on average to make turn")
        print("medium AI spends 10 second on average to make turn")
        print("hard AI spends 20 second on average to make turn")
        print()
        print("extreme AI spends 40 second on average to make turn")
        print()
        print("in custom you can choose how much time will AI in average spend for turn")
        
        tmp = input()
        if tmp == 'easy':
            diff = 5
            break
        elif tmp == 'medium':
            diff = 10
            break
        elif tmp == 'hard':
            diff = 20
            break
        elif tmp == 'extreme':
            diff = 40
            break
        elif tmp == 'custom':
            print("Print how much second on average AI will spend to make turn")
            while True:
                diff = input()
                if isint(diff):
                    diff = int(diff)
                    break
                else:
                    print("Wrong input")
            break
        else:
            print("Wrong input!!!")

print_field()

if mode == 'pvp':
    i = 0
    while check_winner() == '-':
        if i % 2 == 0:
            x_turn()
        else:
            o_turn()
        
        print_field()
        i += 1
    if check_winner() == 'x':
        print("!!! 'x' wins !!!")
    elif check_winner() == 'o':
        print("!!! 'o' wins !!!")
    else:
        print("It's tie. Maybe another match?")
        
elif mode == 'pve':
    print("You will play for 'x' or for 'o'?")
    turn = input()

    print_field()
    if turn == 'x':
        i = 0
        while check_winner() == '-':
            if i % 2 == 0:
                x_turn()
            else:
                o_AI_turn()

            print_field()
            i += 1

        if check_winner() == 'x':
            print("!!! 'x' wins !!! WOW you defeated AI !!! Congratulations !!!")
        elif check_winner() == 'o':
            print(" :( AI wins. Good luck next time")
        else:
            print("It's tie. Maybe another match?")
    elif turn == 'o':
        i = 0
        while check_winner() == '-':
            if i % 2 == 1:
                o_turn()
            else:
                x_AI_turn()

            print_field()
            i += 1

        if check_winner() == 'o':
            print("!!! 'o' wins !!! WOW you defeated AI !!! Congratulations !!!")
        elif check_winner() == 'x':
            print(" :( AI wins. Good luck next time")
        else:
            print("It's tie. Maybe another match?")
elif mode == 'eve':
    i = 0
    while check_winner() == '-':
        if i % 2 == 0:
            x_AI_turn()
        else:
            o_AI_turn()

        print_field()
        i += 1
    if check_winner() == 'x':
        print("!!! 'x' wins !!!")
    elif check_winner() == 'o':
        print("!!! 'o' wins !!!")
    else:
        print("It's tie.")
