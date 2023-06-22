import random

def display_instructions(): #displays the instructions
    print('1) You have to enter the size of the grid between 2-8.')
    print('   For example, if the size of the grid is 3:')
    print('2) It will auto-generate a 3x3 grid with 9 random numbers in it.')
    print('3) Below the grid, it will show the initial scores of two players.')
    print('4) The first player will randomly select a number from a row and column.')
    print('5) The chosen number will be added to the scores.')
    print('6) After selecting the numbers, the selected box will turn into either X or blank.')
    print('7) For now, the second player will select a random number only in the same column as the previous player.')
    print('8) The chosen number will be added to the second player\'s scores.')
    print('9) Now, back to the first player, you can only choose the number in the same row as the previous selected number by the second player.')
    print('10) This game alternates between column and row selection.')
    print('11) The game will end if any row or column is fully taken.')
    print('12) The person with the highest score will win the game!')

def display_scores(player1_name, player1_score, player2_name, player2_score): #displays the names and scores of two player as input and print them on the screen
    print(f"{player1_name}'s Score: {player1_score}")
    print(f"{player2_name}'s Score: {player2_score}")

def get_valid_input(message, options): #get valid user input, it will repeat prompts the user until the valid input is provided #message is the prompt message that will displayed to the user, 'options is a list of valid input options
    while True: #while true loop 
        user_input = input(message).lower() #input() function and stores the input in the 'user_input variable, the lower() method is called on user_input to convert it to lowercase, to ensure that the user's input is case-insentisive 
        if user_input in options: # function check if the user_input is present in the 'options list using the 'in operator
            return user_input #if input is valid, the function immediately return the user-input
        else:
            print('Invalid input. Please enter Y or N.')

def mark_number(grid, row, column): #grid parameter represents the game grid and 'row and 'column represent the coordinates of the cell in the grid that needs to be marked 
    number = grid[row][column] # position of a specific number 
    if number != 'X': # to check wheteher is the value X, if already turned X , then no need to mark again
        grid[row][column] = 'X'

def get_row_with_number(grid, number):  #主要就是要找specific number in the grid
    for row in range(len(grid)):
        for column in range(len(grid[row])):
            if grid[row][column] == number:
                return row
    return None

def player1_turn(grid, player1_name, player1_score, last_selected):
    print(f"{player1_name}, it's your turn.")

    if last_selected == 'column': # this code first checks if the variable 'last selected is equal to the string column
        while True:
            row_input = input(f"{player1_name}, choose a row (a-{chr(ord('a') + len(grid) - 1)}): ") #in while loop, player 1 is asked to choose a row by entering a letter , and the input is stored in the variable row_input
            row = ord(row_input.lower()) - ord('a') # the input is then converted to lowercase to ensure consistency , this step is done to convert the letter into a numerical index , for example, if player 1 enters 'c', the program calculates ord("c") - ord("a") which results in 2 
            if 0 <= row < len(grid):
                break # to check whether if the entered row is valid or not, (less than 0 or greater than or equal to the length of the grid), an error message is printed, and Player 1 is asked to try again.
            else:
                print("Invalid row. Please try again.") # loop continue until player 1 enters a valid row

    while True: # it will keep runing until a valid column choice is made 
        column_input = input(f"{player1_name}, choose a column (1-{len(grid[0])}): ") #grid[0]represents the first row of the grid , len(grid[0]) gives the number of columns in that row 
        column = int(column_input) - 1  # the input is converted to an integer to ensure it is a numerical value # -1 is to adjust it to a zero-based index , this is necessay because lists and arrays in python start counting from 0
        if 0 <= column < len(grid[0]): #The program checks if the calculated column index (column) is within the valid range, from 0 to the length of the grid's columns minus 1
            if grid[row][column] != 'X':
                selected_cell = int(grid[row][column])
                try:
                    selected_number = int(selected_cell) #The program attempts to convert selected_cell to an integer (int(selected_cell)). If the conversion is successful, it means the cell contains a valid number, and the program proceeds.
                    break
                except ValueError: #If the conversion fails (raises a ValueError), it means the cell doesn't contain a valid number, and an error message is printed. Player 1 is asked to try again.
                    print('Invalid number. Please try again.')
            else:
                print("That position is already taken. Please try again.")
        else:
            print("Invalid column. Please try again.")

    player1_score += selected_number #This line increases the score of Player 1 by adding the selected_number to their current score

    print(f"{player1_name} selected the number {selected_number}")
    print(f"{player1_name}'s score: {player1_score}")

    mark_number(grid, row, column) # call that functions 

    return row, player1_score #This line returns the updated row and player1_score values, it can be used by the calling code to keep track of the current state of the game or for further processing.

last_selected = 'row' 
last_row = None #is a special value in Python that represents the absence of a value or the lack of a specific object. In this context, it indicates that there was no previous row selection made.

def player2_turn(grid, player2_name, player2_score, last_row, grid_size, last_selected):
    print(f"{player2_name}, it's your turn.")

    if last_selected == 'row': 
        while True:  
            column_input = input(f"{player2_name}, choose a column (1-{grid_size}): ")
            column = int(column_input) - 1
            if 0 <= column < grid_size:
                break
            else:
                print("Invalid column. Please try again.")
    
    row = 0
    column = -1 # Assign a default value
    
    while True: 
        row_input = input(f"{player2_name}, choose a row (a-{chr(ord('a') + len(grid) - 1)}): ")
        row = ord(row_input.lower()) - ord('a')
        if 0 <= row < len(grid):
            if grid[row][column] != 'X':
                selected_cell = grid[row][column]
                if isinstance(selected_cell, int):
                    selected_number = selected_cell
                elif isinstance(selected_cell, tuple):
                    selected_number = selected_cell[0]
                else:
                    selected_number = None

                if selected_number is not None:
                    grid[row][column] = 'X'
                    player2_score += selected_number
                    break
                else:
                    print("Invalid number. Please try again.")
            else:
                print("That position is already taken. Please try again.")
        else:
            print("Invalid row. Please try again.")

    print(f"{player2_name} selected the number {selected_number}")
    print(f"{player2_name}'s score: {player2_score}")

    mark_number(grid, row, column)

    return column, player2_score


def is_game_over(grid):
    grid_size = len(grid)
    for row in range(grid_size):
        row_complete = all(cell == 'X' for cell in grid[row])
        if row_complete:
            return True

    for col in range(grid_size):
        col_complete = all(grid[row][col] == 'X' for row in range(grid_size))
        if col_complete:
            return True

    return False

def print_grid(grid_size, grid):
    i = 1
    a = 97
    print("  ", end="")
    for x in range(grid_size):
        print(f" {i:^5}", end="")
        i += 1
    print(' ')
    print("  ", end="")
    print('-' * ((grid_size * 6) + 1))
    for rows in grid:
        print(f"{chr(a)} ", end="")
        a += 1
        for cols in rows:
            print(f"|{cols:^5}", end="")
        print("|")
        print("  ", end='')
        print('-' * ((grid_size * 6) + 1))

def initialise_game(grid, grid_size):
    p1 = 0
    p2 = 0
    random_rows = random.randint(0, grid_size - 1)
    random_cols = random. randint(0, grid_size - 1)
    p1 += grid[random_rows][random_cols] #把那个selected box放进分数
    grid[random_rows][random_cols] = 'x'#才变x
    return grid, p1, p2



def start_game():
    global last_row

    instruction = get_valid_input('Do you need instructions? Y / N: ', ['y', 'n'])

    if instruction == 'y':
        display_instructions()

    question = get_valid_input('Do you want to start the game? Y / N: ', ['y', 'n'])

    if question == 'y':
        grid_size = int(input('Enter the grid size you want: '))

        grid = [[random.randint(-30, 30) for _ in range(grid_size)] for _ in range(grid_size)]
        col_width = max(len(str(cell)) for row in grid for cell in row)
        col, row = grid_size, grid_size

        # print(' ' + '------' * grid_size)

        print_grid(grid_size, grid)

        player1_name = input("Enter name for Player 1: ")
        player2_name = input("Enter name for Player 2: ")
        grid, player1_score, player2_score = initialise_game(grid, grid_size)
        print_grid(grid_size, grid)

        # # Initialize last_selected variable
        last_row = None
        last_selected = 'column'

        # print_grid(grid_size, grid)

        while True:
            # Display player names and scores
            display_scores(player1_name, player1_score, player2_name, player2_score)

            if last_selected == 'column':
                row, player2_score = player2_turn(grid, player2_name, player2_score,last_row,len(grid), last_selected)
                last_selected = 'row'
            else:
                player1_score = player1_turn(grid, player1_name, player1_score, last_row, grid_size, last_selected)
                last_selected = 'column'

            if is_game_over(grid):
                break

            print_grid(grid_size, grid)
            # print(' ' * (col_width + 4), end='')
            # for col in range(grid_size):
            #     print(f'{col+1:^{col_width}}', end=' ')
            # print('\n' + '  -' * ((col_width + 3) * grid_size))

            # for row in range(grid_size):
            #     print(chr(97+row), end=' | ')
            #     for col in range(grid_size):
            #         print(f'{grid[row][col]:^{col_width}}', end=' | ')
            #     print('\n' + '  -' * ((col_width + 3) * grid_size))

        for row in grid:
            print('+', end='')
            for _ in range(col_width + 3):
                print('-', end='')
            print('+', end='')
        print()

        if player1_score > player2_score:
            print(f"{player1_name} wins!")
        elif player2_score > player1_score:
            print(f"{player2_name} wins!")
        else:
            print("It's a tie!")

    print()

start_game()

