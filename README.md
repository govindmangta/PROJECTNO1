import pygame
import sys

# Initialize Pygame
pygame.init()

# Screen dimensions
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption('Bouncing Ball Game')

# Ball settings
ball_radius = 30
ball_color = (255, 87, 51)
ball_x = screen_width // 2
ball_y = screen_height // 2
ball_dx = 5
ball_dy = 5

# Paddle settings
paddle_width = 100
paddle_height = 20
paddle_color = (0, 149, 221)
paddle_x = (screen_width - paddle_width) // 2

# Game loop
clock = pygame.time.Clock()
while True:
    screen.fill((240, 240, 240))  # Background color

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Ball movement
    ball_x += ball_dx
    ball_y += ball_dy

    # Ball collision with walls
    if ball_x + ball_radius > screen_width or ball_x - ball_radius < 0:
        ball_dx = -ball_dx
    if ball_y - ball_radius < 0:
        ball_dy = -ball_dy
    if ball_y + ball_radius > screen_height - paddle_height:
        if paddle_x < ball_x < paddle_x + paddle_width:
            ball_dy = -ball_dy
        else:
            pygame.quit()
            sys.exit()  # Exit if the ball hits the ground

    # Paddle movement (using arrow keys)
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and paddle_x > 0:
        paddle_x -= 10
    if keys[pygame.K_RIGHT] and paddle_x < screen_width - paddle_width:
        paddle_x += 10

    # Draw the ball
    pygame.draw.circle(screen, ball_color, (ball_x, ball_y), ball_radius)

    # Draw the paddle
    pygame.draw.rect(screen, paddle_color, (paddle_x, screen_height - paddle_height, paddle_width, paddle_height))

    # Update the screen
    pygame.display.flip()

    # Set the game frame rate
    clock.tick(60)
