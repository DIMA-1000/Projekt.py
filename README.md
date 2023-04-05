import pygame
import random

# инициализация Pygame
pygame.init()

# определение цветов
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# определение размеров экрана
SCREEN_WIDTH = 600
SCREEN_HEIGHT = 400

# создание экрана
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

# задержка обновления экрана
clock = pygame.time.Clock()

# определение размеров змейки
BLOCK_SIZE = 10

# функция отображения текста на экране
def draw_text(surface, text, size, x, y):
    font = pygame.font.Font('freesansbold.ttf', size)
    text_surface = font.render(text, True, WHITE)
    text_rect = text_surface.get_rect()
    text_rect.center = (x, y)
    surface.blit(text_surface, text_rect)

# функция отображения змейки
def draw_snake(surface, snake):
    for block in snake:
        pygame.draw.rect(surface, GREEN, [block[0], block[1], BLOCK_SIZE, BLOCK_SIZE])

# функция генерации еды
def generate_food(snake):
    while True:
        x = random.randrange(0, SCREEN_WIDTH-BLOCK_SIZE, BLOCK_SIZE)
        y = random.randrange(0, SCREEN_HEIGHT-BLOCK_SIZE, BLOCK_SIZE)
        food_rect = pygame.Rect(x, y, BLOCK_SIZE, BLOCK_SIZE)
        if food_rect.collidelist(snake) == -1:
            return food_rect

# определение начального положения змейки и еды
snake = [[SCREEN_WIDTH//2, SCREEN_HEIGHT//2]]
food = generate_food(snake)

# определение направления движения змейки
dx = BLOCK_SIZE
dy = 0

# главный игровой цикл
game_over = False
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT and dx == 0:
                dx = -BLOCK_SIZE
                dy = 0
            elif event.key == pygame.K_RIGHT and dx == 0:
                dx = BLOCK_SIZE
                dy = 0
            elif event.key == pygame.K_UP and dy == 0:
                dx = 0
                dy = -BLOCK_SIZE
            elif event.key == pygame.K_DOWN and dy == 0:
                dx = 0
                dy = BLOCK_SIZE

    # движение змейки
    head = [snake[0][0]+dx, snake[0][1]+dy]
    snake.insert(0, head)
    if head == food.topleft:
        food = generate_food(snake)
    else:
        snake.pop()

    # проверка на столкновение с границами экрана
    if snake[0][0] < 0 or snake[0][0] >= SCREEN_WIDTH or snake[0][1] < 0 or snake[0][1] >= SCREEN_HEIGHT:
        game_over = True

    # проверка на столк
