import pygame
import sys
import random

pygame.init()

# ================== WINDOW ==================
WIDTH, HEIGHT = 900, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Ping Pong Ultimate v1.0")
clock = pygame.time.Clock()

# ================== FONTS ==================
font = pygame.font.SysFont("Courier", 22)
big_font = pygame.font.SysFont("Courier", 46)
small_font = pygame.font.SysFont("Courier", 16)

# ================== COLORS ==================
WHITE = (255, 255, 255)
BLACK = (20, 20, 20)
GRAY = (140, 140, 140)

COLORS_STORE = {
    "Blue":   {"rgb": (0, 120, 255), "price": 100},
    "Red":    {"rgb": (255, 70, 70), "price": 130},
    "Green":  {"rgb": (0, 200, 120), "price": 160},
    "Yellow": {"rgb": (255, 230, 0), "price": 190},
    "Purple": {"rgb": (170, 80, 255), "price": 220},
    "Orange": {"rgb": (255, 150, 0), "price": 260},
}

# ================== STATES ==================
MENU = "menu"
STORE = "store"
GAME = "game"
PAUSE = "pause"
HOW = "how"
CPU_SELECT = "cpu_select"
CONFIRM_COLOR = "confirm_color"
RESULT = "result"

state = MENU

# ================== PLAYER DATA ==================
owned_colors = ["Blue"]
selected_color = "Blue"
pending_color = None
coins = 150

# ================== GAME DATA ==================
game_mode = "PVP"
cpu_level = "Easy"
WIN_SCORE = 10
winner_text = ""

# ================== OBJECTS ==================
ball = pygame.Rect(WIDTH//2 - 10, HEIGHT//2 - 10, 20, 20)
player1 = pygame.Rect(40, HEIGHT//2 - 60, 15, 120)
player2 = pygame.Rect(WIDTH - 55, HEIGHT//2 - 60, 15, 120)

ball_dx = random.choice([-6, 6])
ball_dy = random.choice([-5, 5])
player_speed = 7

p1_score = 0
p2_score = 0

# ================== HELPERS ==================
def draw_text(text, f, color, x, y):
    img = f.render(text, True, color)
    screen.blit(img, (x - img.get_width()//2, y))

def reset_game():
    global p1_score, p2_score, ball_dx, ball_dy
    p1_score = 0
    p2_score = 0
    ball.center = (WIDTH//2, HEIGHT//2)
    ball_dx = random.choice([-6, 6])
    ball_dy = random.choice([-5, 5])
    player1.centery = HEIGHT//2
    player2.centery = HEIGHT//2

def reward_player():
    global coins
    rewards = {"Easy": 40, "Medium": 80, "Hard": 150}
    coins += rewards[cpu_level]

# ================== SCREENS ==================
def draw_menu():
    screen.fill(BLACK)
    draw_text("PING PONG ULTIMATE", big_font, WHITE, WIDTH//2, 80)
    draw_text("1 - Player vs Computer", font, WHITE, WIDTH//2, 220)
    draw_text("2 - Two Players", font, WHITE, WIDTH//2, 260)
    draw_text("S - Store", font, WHITE, WIDTH//2, 300)
    draw_text("H - How To Play", font, WHITE, WIDTH//2, 340)
    draw_text("ESC - Exit", font, WHITE, WIDTH//2, 380)
    draw_text("Made by Sheeba", small_font, GRAY, WIDTH//2, HEIGHT-30)
    pygame.display.update()

def draw_cpu_select():
    screen.fill(BLACK)
    draw_text("SELECT DIFFICULTY", big_font, WHITE, WIDTH//2, 120)
    draw_text("1 - Easy", font, WHITE, WIDTH//2, 220)
    draw_text("2 - Medium", font, WHITE, WIDTH//2, 260)
    draw_text("3 - Hard", font, WHITE, WIDTH//2, 300)
    draw_text("B - Back", font, WHITE, WIDTH//2, 360)
    pygame.display.update()

def draw_store():
    screen.fill(BLACK)
    draw_text("STORE", big_font, WHITE, WIDTH//2, 60)
    draw_text(f"Coins: {coins}", font, WHITE, WIDTH//2, 120)
    y = 180
    for name, data in COLORS_STORE.items():
        status = "OWNED" if name in owned_colors else f"{data['price']} COINS"
        draw_text(f"{name} - {status}", font, data["rgb"], WIDTH//2, y)
        y += 35
    draw_text("Press first letter to buy/select", font, WHITE, WIDTH//2, 500)
    draw_text("B - Back", font, WHITE, WIDTH//2, 530)
    pygame.display.update()

def draw_confirm_color():
    screen.fill(BLACK)
    draw_text(f"Play with {pending_color} ?", big_font, WHITE, WIDTH//2, 220)
    draw_text("Y - Yes", font, WHITE, WIDTH//2, 300)
    draw_text("N - No", font, WHITE, WIDTH//2, 340)
    pygame.display.update()

def draw_how():
    screen.fill(BLACK)
    draw_text("HOW TO PLAY", big_font, WHITE, WIDTH//2, 80)
    draw_text("W / S  : Player 1", font, WHITE, WIDTH//2, 200)
    draw_text("UP / DOWN : Player 2", font, WHITE, WIDTH//2, 240)
    draw_text("P : Pause", font, WHITE, WIDTH//2, 280)
    draw_text("First to 10 Wins", font, WHITE, WIDTH//2, 320)
    draw_text("B - Back", font, WHITE, WIDTH//2, 380)
    pygame.display.update()

def draw_pause():
    screen.fill(BLACK)
    draw_text("PAUSED", big_font, WHITE, WIDTH//2, 220)
    draw_text("R - Resume", font, WHITE, WIDTH//2, 300)
    draw_text("M - Main Menu", font, WHITE, WIDTH//2, 340)
    pygame.display.update()

def draw_result():
    screen.fill(BLACK)
    draw_text(winner_text, big_font, WHITE, WIDTH//2, 200)
    draw_text("R - Rematch", font, WHITE, WIDTH//2, 320)
    draw_text("M - Main Menu", font, WHITE, WIDTH//2, 360)
    pygame.display.update()

def draw_game():
    screen.fill(BLACK)
    for y in range(0, HEIGHT, 20):
        pygame.draw.rect(screen, WHITE, (WIDTH//2 - 2, y, 4, 10))
    pygame.draw.rect(screen, COLORS_STORE[selected_color]["rgb"], player1)
    pygame.draw.rect(screen, (255, 70, 70), player2)
    pygame.draw.ellipse(screen, WHITE, ball)
    draw_text(f"{p1_score} : {p2_score}", font, WHITE, WIDTH//2, 20)
    pygame.display.update()

# ================== MAIN LOOP ==================
while True:
    clock.tick(60)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        if event.type == pygame.KEYDOWN:

            if state == MENU:
                if event.key == pygame.K_1:
                    state = CPU_SELECT
                elif event.key == pygame.K_2:
                    game_mode = "PVP"
                    reset_game()
                    state = GAME
                elif event.key == pygame.K_s:
                    state = STORE
                elif event.key == pygame.K_h:
                    state = HOW
                elif event.key == pygame.K_ESCAPE:
                    pygame.quit()
                    sys.exit()

            elif state == CPU_SELECT:
                if event.key == pygame.K_1:
                    cpu_level = "Easy"
                elif event.key == pygame.K_2:
                    cpu_level = "Medium"
                elif event.key == pygame.K_3:
                    cpu_level = "Hard"
                elif event.key == pygame.K_b:
                    state = MENU
                    continue
                game_mode = "CPU"
                reset_game()
                state = GAME

            elif state == STORE:
                if event.key == pygame.K_b:
                    state = MENU
                    continue
                if event.unicode.isalpha():
                    for name, data in COLORS_STORE.items():
                        if event.unicode.lower() == name[0].lower():
                            if name not in owned_colors:
                                if coins >= data["price"]:
                                    coins -= data["price"]
                                    owned_colors.append(name)
                                else:
                                    break
                            pending_color = name
                            state = CONFIRM_COLOR
                            break

            elif state == CONFIRM_COLOR:
                if event.key == pygame.K_y:
                    selected_color = pending_color
                    state = STORE
                elif event.key == pygame.K_n:
                    state = STORE

            elif state == HOW:
                if event.key == pygame.K_b:
                    state = MENU

            elif state == PAUSE:
                if event.key == pygame.K_r:
                    state = GAME
                elif event.key == pygame.K_m:
                    state = MENU

            elif state == RESULT:
                if event.key == pygame.K_r:
                    reset_game()
                    state = GAME
                elif event.key == pygame.K_m:
                    state = MENU

    if state == GAME:
        keys = pygame.key.get_pressed()

        if keys[pygame.K_p]:
            state = PAUSE

        if keys[pygame.K_w] and player1.top > 0:
            player1.y -= player_speed
        if keys[pygame.K_s] and player1.bottom < HEIGHT:
            player1.y += player_speed

        if game_mode == "CPU":
            speed = {"Easy": 5, "Medium": 8, "Hard": 12}[cpu_level]
            if player2.centery < ball.centery:
                player2.y += speed
            if player2.centery > ball.centery:
                player2.y -= speed
        else:
            if keys[pygame.K_UP] and player2.top > 0:
                player2.y -= player_speed
            if keys[pygame.K_DOWN] and player2.bottom < HEIGHT:
                player2.y += player_speed

        ball.x += ball_dx
        ball.y += ball_dy

        if ball.top <= 0 or ball.bottom >= HEIGHT:
            ball_dy *= -1

        if ball.colliderect(player1) or ball.colliderect(player2):
            ball_dx *= -1

        if ball.left <= 0:
            p2_score += 1
            ball.center = (WIDTH//2, HEIGHT//2)

        if ball.right >= WIDTH:
            p1_score += 1
            ball.center = (WIDTH//2, HEIGHT//2)

        if p1_score == WIN_SCORE or p2_score == WIN_SCORE:
            if game_mode == "CPU" and p1_score > p2_score:
                reward_player()
                winner_text = "YOU WIN!"
            else:
                winner_text = "YOU LOSE!" if game_mode == "CPU" else "PLAYER 2 WINS!"
            state = RESULT

        draw_game()

    elif state == MENU:
        draw_menu()
    elif state == STORE:
        draw_store()
    elif state == CPU_SELECT:
        draw_cpu_select()
    elif state == CONFIRM_COLOR:
        draw_confirm_color()
    elif state == HOW:
        draw_how()
    elif state == PAUSE:
        draw_pause()
    elif state == RESULT:
        draw_result()
