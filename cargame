import pygame
import random
import os
import sys

# Initialize
pygame.init()
pygame.mixer.init()

# Screen settings
WIDTH, HEIGHT = 400, 600
win = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Car Game")

# Clock
clock = pygame.time.Clock()
FPS = 60

# Load images
car_img = pygame.image.load("assets/car.png")
car_img = pygame.transform.scale(car_img, (50, 100))

enemy_car_img = pygame.image.load("assets/enemy_car.png")
enemy_car_img = pygame.transform.scale(enemy_car_img, (50, 100))

coin_img = pygame.image.load("assets/coin.png")
coin_img = pygame.transform.scale(coin_img, (30, 30))

obstacle_img = pygame.image.load("assets/obstacle.png")
obstacle_img = pygame.transform.scale(obstacle_img, (35, 35))

boost_img = pygame.image.load("assets/boost.png")
boost_img = pygame.transform.scale(boost_img, (30, 30))

slow_img = pygame.image.load("assets/slow.png")
slow_img = pygame.transform.scale(slow_img, (30, 30))

# Load sounds
coin_sound = pygame.mixer.Sound("assets/sounds/coin.wav")
crash_sound = pygame.mixer.Sound("assets/sounds/crash.wav")
boost_sound = pygame.mixer.Sound("assets/sounds/boost.wav")
slow_sound = pygame.mixer.Sound("assets/sounds/slow.wav")

# Background music
pygame.mixer.music.load("assets/sounds/background.mp3")
pygame.mixer.music.set_volume(0.5)
pygame.mixer.music.play(-1)

# Player
car_x, car_y = WIDTH // 2 - 25, HEIGHT - 120
car_speed = 5

# Enemy cars
enemy_cars = []
def spawn_enemy():
    x = random.randint(0, WIDTH - 50)
    y = -100
    speed = random.randint(3, 6)
    return {'x': x, 'y': y, 'speed': speed}
for _ in range(2):
    enemy_cars.append(spawn_enemy())

# Coins, obstacles, boosts, slows
coin = {'x': random.randint(0, WIDTH - 30), 'y': -100}
obstacle = {'x': random.randint(0, WIDTH - 35), 'y': -500, 'speed': 3}
boost = {'x': random.randint(0, WIDTH - 30), 'y': -300}
slow = {'x': random.randint(0, WIDTH - 30), 'y': -700}

# Score
score = 0
high_score = 0
if os.path.exists("highscore.txt"):
    with open("highscore.txt", "r") as file:
        high_score = int(file.read().strip())

# Game state
run = True
font = pygame.font.SysFont(None, 30)

def draw_text(text, x, y, color=(255,255,255)):
    render = font.render(text, True, color)
    win.blit(render, (x, y))

while run:
    clock.tick(FPS)
    win.fill((30, 30, 30))

    # Events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False

    # Movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and car_x > 0:
        car_x -= car_speed
    if keys[pygame.K_RIGHT] and car_x < WIDTH - 50:
        car_x += car_speed

    # Enemy movement
    for car in enemy_cars:
        car['y'] += car['speed']
        if car['y'] > HEIGHT:
            car.update(spawn_enemy())
            score += 1
        win.blit(enemy_car_img, (car['x'], car['y']))
        if car_x < car['x'] + 50 and car_x + 50 > car['x'] and car_y < car['y'] + 100 and car_y + 100 > car['y']:
            crash_sound.play()
            run = False

    # Coin
    coin['y'] += 5
    if coin['y'] > HEIGHT:
        coin = {'x': random.randint(0, WIDTH - 30), 'y': -100}
    if car_x < coin['x'] + 30 and car_x + 50 > coin['x'] and car_y < coin['y'] + 30 and car_y + 100 > coin['y']:
        coin_sound.play()
        score += 5
        coin = {'x': random.randint(0, WIDTH - 30), 'y': -100}
    win.blit(coin_img, (coin['x'], coin['y']))

    # Obstacle
    obstacle['y'] += obstacle['speed']
    if obstacle['y'] > HEIGHT:
        obstacle = {'x': random.randint(0, WIDTH - 35), 'y': -500, 'speed': 3}
    if car_x < obstacle['x'] + 35 and car_x + 50 > obstacle['x'] and car_y < obstacle['y'] + 35 and car_y + 100 > obstacle['y']:
        crash_sound.play()
        run = False
    win.blit(obstacle_img, (obstacle['x'], obstacle['y']))

    # Boost
    boost['y'] += 5
    if boost['y'] > HEIGHT:
        boost = {'x': random.randint(0, WIDTH - 30), 'y': -300}
    if car_x < boost['x'] + 30 and car_x + 50 > boost['x'] and car_y < boost['y'] + 30 and car_y + 100 > boost['y']:
        boost_sound.play()
        car_speed += 2
        boost = {'x': random.randint(0, WIDTH - 30), 'y': -300}
    win.blit(boost_img, (boost['x'], boost['y']))

    # Slow
    slow['y'] += 4
    if slow['y'] > HEIGHT:
        slow = {'x': random.randint(0, WIDTH - 30), 'y': -700}
    if car_x < slow['x'] + 30 and car_x + 50 > slow['x'] and car_y < slow['y'] + 30 and car_y + 100 > slow['y']:
        slow_sound.play()
        car_speed = max(3, car_speed - 2)
        slow = {'x': random.randint(0, WIDTH - 30), 'y': -700}
    win.blit(slow_img, (slow['x'], slow['y']))

    # Draw player
    win.blit(car_img, (car_x, car_y))

    # Score
    draw_text(f"Score: {score}", 10, 10)
    draw_text(f"High Score: {high_score}", 10, 40)

    pygame.display.update()

# Save high score
if score > high_score:
    with open("highscore.txt", "w") as file:
        file.write(str(score))

# Show final screen
win.fill((0, 0, 0))
draw_text(f"Game Over!", WIDTH//2 - 60, HEIGHT//2 - 30)
draw_text(f"Score: {score}", WIDTH//2 - 50, HEIGHT//2)
draw_text(f"High Score: {max(score, high_score)}", WIDTH//2 - 80, HEIGHT//2 + 30)
draw_text("Press R to Restart or Q to Quit", WIDTH//2 - 120, HEIGHT//2 + 70)
pygame.display.update()

# Wait for restart or quit
waiting = True
while waiting:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            waiting = False
        keys = pygame.key.get_pressed()
        if keys[pygame.K_r]:
           os.execv(sys.executable, ['python', 'main.py'])
        if keys[pygame.K_q]:
            waiting = False

pygame.quit()
