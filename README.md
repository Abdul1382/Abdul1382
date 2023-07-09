import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the game window
width, height = 640, 480
window = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Define colors
black = (0, 0, 0)
white = (255, 255, 255)
green = (0, 255, 0)
red = (255, 0, 0)

# Snake initial position and size
snake_block = 10
snake_speed = 15
x, y = width // 2, height // 2
x_change, y_change = 0, 0

# Create the snake body
snake_body = []
snake_length = 1

# Create the food position
food_x = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
food_y = round(random.randrange(0, height - snake_block) / 10.0) * 10.0

clock = pygame.time.Clock()

game_over = False

# Game loop
while not game_over:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                x_change = -snake_block
                y_change = 0
            elif event.key == pygame.K_RIGHT:
                x_change = snake_block
                y_change = 0
            elif event.key == pygame.K_UP:
                y_change = -snake_block
                x_change = 0
            elif event.key == pygame.K_DOWN:
                y_change = snake_block
                x_change = 0

    # Update snake position
    x += x_change
    y += y_change

    # Check if the snake hits the boundaries
    if x >= width or x < 0 or y >= height or y < 0:
        game_over = True

    # Update the game window
    window.fill(black)
    pygame.draw.rect(window, green, [food_x, food_y, snake_block, snake_block])

    snake_head = [x, y]
    snake_body.append(snake_head)

    if len(snake_body) > snake_length:
        del snake_body[0]

    for segment in snake_body[:-1]:
        if segment == snake_head:
            game_over = True

    for segment in snake_body:
        pygame.draw.rect(window, white, [segment[0], segment[1], snake_block, snake_block])

    pygame.display.update()

    # Check if the snake eats the food
    if x == food_x and y == food_y:
        food_x = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
        food_y = round(random.randrange(0, height - snake_block) / 10.0) * 10.0
        snake_length += 1

    # Set the game speed
    clock.tick(snake_speed)

# Quit the game
pygame.quit()

