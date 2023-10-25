- 👋 Hi, I’m @ducaca2
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
ducaca2/ducaca2 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random

# Khởi tạo Pygame
pygame.init()

# Các biến cơ bản
WIDTH, HEIGHT = 800, 600
SNAKE_SIZE = 20
SPEED = 15

# Màu sắc
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)

# Màn hình chơi game
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Định nghĩa hàm vẽ con rắn
def draw_snake(snake_body):
    for segment in snake_body:
        pygame.draw.rect(screen, GREEN, [segment[0], segment[1], SNAKE_SIZE, SNAKE_SIZE])

# Hàm chạy trò chơi
def game():
    game_over = False
    game_close = False

    x, y = WIDTH // 2, HEIGHT // 2
    x_change, y_change = 0, 0

    snake_body = []
    length_of_snake = 1

    # Tạo thức ăn ngẫu nhiên
    food_x = round(random.randrange(0, WIDTH - SNAKE_SIZE) / SNAKE_SIZE) * SNAKE_SIZE
    food_y = round(random.randrange(0, HEIGHT - SNAKE_SIZE) / SNAKE_SIZE) * SNAKE_SIZE

    while not game_over:

        while game_close == True:
            screen.fill(BLACK)
            font = pygame.font.Font(None, 36)
            text = font.render("You lost! Press Q-Quit or C-Play Again", True, WHITE)
            screen.blit(text, (200, 250))
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        game()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -SNAKE_SIZE
                    y_change = 0
                elif event.key == pygame.K_RIGHT:
                    x_change = SNAKE_SIZE
                    y_change = 0
                elif event.key == pygame.K_UP:
                    x_change = 0
                    y_change = -SNAKE_SIZE
                elif event.key == pygame.K_DOWN:
                    x_change = 0
                    y_change = SNAKE_SIZE

        if x >= WIDTH or x < 0 or y >= HEIGHT or y < 0:
            game_close = True
        x += x_change
        y += y_change
        screen.fill(BLACK)
        pygame.draw.rect(screen, WHITE, [food_x, food_y, SNAKE_SIZE, SNAKE_SIZE])
        snake_head = []
        snake_head.append(x)
        snake_head.append(y)
        snake_body.append(snake_head)
        if len(snake_body) > length_of_snake:
            del snake_body[0]

        for segment in snake_body[:-1]:
            if segment == snake_head:
                game_close = True

        draw_snake(snake_body)
        pygame.display.update()

        if x == food_x and y == food_y:
            food_x = round(random.randrange(0, WIDTH - SNAKE_SIZE) / SNAKE_SIZE) * SNAKE_SIZE
            food_y = round(random.randrange(0, HEIGHT - SNAKE_SIZE) / SNAKE_SIZE) * SNAKE_SIZE
            length_of_snake += 1

        pygame.time.delay(SPEED)

    pygame.quit()
    quit()

# Bắt đầu trò chơi
game()
