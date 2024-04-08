import random
import tkinter


def move_wrap(obj, move):
    global N_X, N_Y, step
    coords = canvas.coords(obj)
    x, y = coords[0], coords[1]
    new_x = (x + move[0]) % (N_X * step)
    new_y = (y + move[1]) % (N_Y * step)
    canvas.coords(obj, new_x, new_y, new_x + step, new_y + step)
    check_move()


def check_move():
    player_coords = canvas.coords(player)
    enemy_coords = canvas.coords(enemy)
    if player_coords == enemy_coords:
        label.config(text="Поражение!")
    elif player_coords == canvas.coords(exit):
        label.config(text="Победа!")


def key_pressed(event):
    global freeze_countdown
    if event.keysym == 'Up':
        move_wrap(player, (0, -step))
    elif event.keysym == 'Down':
        move_wrap(player, (0, step))
    elif event.keysym == 'Left':
        move_wrap(player, (-step, 0))
    elif event.keysym == 'Right':
        move_wrap(player, (step, 0))
    elif event.keysym == 'space' and freeze_countdown == 0:
        freeze_enemies()


def move_enemy(obj):
    global freeze_countdown
    if freeze_countdown == 0:
        player_coords = canvas.coords(player)
        enemy_coords = canvas.coords(obj)
        enemy_x, enemy_y = enemy_coords[0], enemy_coords[1]
        player_x, player_y = player_coords[0], player_coords[1]

        diff_x = player_x - enemy_x
        diff_y = player_y - enemy_y

        if abs(diff_x) > abs(diff_y):
            if diff_x > 0:
                move = (step, 0)
            else:
                move = (-step, 0)
        else:
            if diff_y > 0:
                move = (0, step)
            else:
                move = (0, -step)

        move_wrap(obj, move)

    master.after(1000, move_enemy, obj)


def freeze_enemies():
    global freeze_countdown
    freeze_countdown = 3
    label.config(text="Враги заморожены")
    master.after(2000, unfreeze_enemies)


def unfreeze_enemies():
    global freeze_countdown
    freeze_countdown = 0
    label.config(text="Найди выход")


def decrement_freeze_countdown():
    global freeze_countdown
    if freeze_countdown > 0:
        freeze_countdown -= 1
        if freeze_countdown == 0:
            label.config(text="Найди выход")
        else:
            label.config(text=f"Враги заморожены ({freeze_countdown} хода)")
    master.after(1000, decrement_freeze_countdown)


master = tkinter.Tk()

step = 60
N_X = 10
N_Y = 10
canvas = tkinter.Canvas(master, bg='blue',
                        width=step * N_X, height=step * N_Y)

player_pos = (random.randint(0, N_X - 1) * step,
              random.randint(0, N_Y - 1) * step)
exit_pos = (random.randint(0, N_X - 1) * step,
            random.randint(0, N_Y - 1) * step)

player = canvas.create_oval((player_pos[0], player_pos[1]),
                            (player_pos[0] + step, player_pos[1] + step),
                            fill='green')

exit = canvas.create_oval((exit_pos[0], exit_pos[1]),
                          (exit_pos[0] + step, exit_pos[1] + step),
                          fill='yellow')

enemy_pos = (random.randint(0, N_X - 1) * step,
             random.randint(0, N_Y - 1) * step)

enemy = canvas.create_oval((enemy_pos[0], enemy_pos[1]),
                           (enemy_pos[0] + step, enemy_pos[1] + step), fill='red')

label = tkinter.Label(master, text="Найди выход")
label.pack()
canvas.pack()

master.bind("<KeyPress>", key_pressed)

freeze_countdown = 0

move_enemy(enemy)

master.after(1000, decrement_freeze_countdown)

master.mainloop()

