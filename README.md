import random


class TicTacToe:
    FREE_CELL = 0  # свободная клетка
    HUMAN_X = 1  # крестик (игрок - человек)
    COMPUTER_O = 2

    def __init__(self):
        self.pole = tuple(tuple(Cell() for _ in range(3)) for _ in range(3))

    def check__index(self, indx):
        i, j = indx
        if not 0 <= i <= 2 or not 0 <= j <= 2:
            raise IndexError('некорректно указанные индексы')

    def __getitem__(self, item):
        self.check__index(item)
        i, j = item
        return self.pole[i][j].value

    def __setitem__(self, key, value):
        self.check__index(key)
        i, j = key
        self.pole[i][j].value = value

    def init(self):
        for i in range(3):
            for j in range(3):
                self.pole[i][j].value = self.FREE_CELL

    def show(self):
        for i in self.pole:
            for j in i:
                if j.value == 1:
                    print("X", end=" ")
                elif j.value == 2:
                    print("O", end=" ")
                else:
                    print("-", end=" ")
            print()
        print("*****")

    def human_go(self):
        while True:
            if self.is_computer_win or self.is_human_win:
                break
            else:
                i, j = map(int, input().split())
                if self.pole[i][j].value == self.FREE_CELL:
                    self.pole[i][j].value = self.HUMAN_X
                    return
                else:
                    return self.human_go()

    def computer_go(self):
        while True:
            if self.is_computer_win or self.is_human_win:
                break
            else:
                i = random.randint(0, 2)
                j = random.randint(0, 2)
                if self.pole[i][j].value == self.FREE_CELL:
                    self.pole[i][j].value = self.COMPUTER_O
                    return
                else:
                    return self.computer_go()

    @property
    def is_human_win(self):
        if all(self.pole[i][i].value == self.HUMAN_X for i in range(3)):
            return True
        if all(self.pole[i][~i].value == self.HUMAN_X for i in range(3)):
            return True
        for i in self.pole:
            if all(map(lambda x: x.value == self.HUMAN_X, i)):
                return True
        for i in zip(*self.pole):
            if all(map(lambda x: x.value == self.HUMAN_X, i)):
                return True

        return False

    @property
    def is_computer_win(self):
        for i in self.pole:
            if all(map(lambda x: x.value == self.COMPUTER_O, i)):
                return True
        for i in zip(*self.pole):
            if all(map(lambda x: x.value == self.COMPUTER_O, i)):
                return True

        if all(self.pole[i][i].value == self.COMPUTER_O for i in range(3)):
            return True
        if all(self.pole[i][~i].value == self.COMPUTER_O for i in range(3)):
            return True
        return False

    @property
    def is_draw(self):
        if all(j.value == 0 for i in self.pole for j in i):
            return False
        if self.is_human_win or self.is_computer_win:
            return False
        for i in self.pole:
            for j in i:
                if j.value != self.FREE_CELL:
                    return True
        return False

    def __bool__(self):
        if self.is_computer_win:
            return False
        if self.is_human_win:
            return False
        for i in self.pole:
            for j in i:
                if j.value == self.FREE_CELL:
                    return True
        else:
            return False


class Cell:
    def __init__(self):
        self.value = 0

    def __bool__(self):
        return self.value == 0
