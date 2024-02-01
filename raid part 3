import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the game window
width, height = 460, 307
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Escape the Police")

# Placeholder functions
def load_and_scale_image(image_path, size):
    image = pygame.image.load(image_path)
    return pygame.transform.scale(image, size)

def calculate_distance(point1, point2):
    return ((point1[0] - point2[0]) ** 2 + (point1[1] - point2[1]) ** 2) ** 0.5

# Load police and player characters
police_image = load_and_scale_image("police.png", (50, 50))
player_image = load_and_scale_image("player.png", (50, 50))
shed_image = load_and_scale_image("shed.png", (100, 100))

police_x, police_y = 400, 140
police_speed = 0.25

# Define shed area
shed_rect = pygame.Rect(300, 50, 100, 100)

# Modify objective text
objective_text = "Objective: Escape the Police and Find Refuge in the Shed"

# Placeholder variables
player_x, player_y = 50, 140
game_active = True
dialogue_active = False
key_pressed = False
selected_answer = 0
dialogue_window_active = False

# Police chase logic
def police_chase():
    global police_x, police_y, player_x, player_y, game_active

    # Implement chase logic with a more reasonable speed
    speed_factor = 1.5
    if player_x < police_x:
        police_x -= police_speed * speed_factor
    elif player_x > police_x:
        police_x += police_speed * speed_factor

    if player_y < police_y:
        police_y -= police_speed * speed_factor
    elif player_y > police_y:
        police_y += police_speed * speed_factor

    # Check if the police caught the player
    if calculate_distance((player_x, player_y), (police_x, police_y)) < 20:
        print("Game Over - The police caught you!")
        game_active = False

# Shed interaction logic
def enter_shed():
    global game_active

    if shed_rect.colliderect(pygame.Rect(player_x, player_y, 50, 50)): # Adjust the player size
        print("You've escaped into the shed!")
        game_active = False

# Main game loop modifications
while game_active and not dialogue_window_active:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if not dialogue_active:
                key_pressed = True
                if event.key == pygame.K_LEFT:
                    player_x -= 5
                elif event.key == pygame.K_RIGHT:
                    player_x += 5
                elif event.key == pygame.K_UP:
                    player_y -= 5
                elif event.key == pygame.K_DOWN:
                    player_y += 5
                elif event.key == pygame.K_RETURN:
                    # Enter dialogue mode and open the dialogue window
                    dialogue_active = True
                    selected_answer = 0
                    dialogue_window_active = True
        elif event.type == pygame.KEYUP:
            key_pressed = False

    keys = pygame.key.get_pressed()

    # Police chase logic
    police_chase()

    # Shed interaction logic
    enter_shed()

    # Update the display
    screen.fill((255, 255, 255)) # Fill the screen with a white background

    # Draw the police character
    screen.blit(police_image, (police_x, police_y))

    # Draw the player character
    screen.blit(player_image, (player_x, player_y))

    # Draw the shed area
    screen.blit(shed_image, (shed_rect.x, shed_rect.y))

    # Draw the objective text
    font = pygame.font.Font(None, 36)
    text = font.render(objective_text, True, (0, 0, 0))
    screen.blit(text, (10, 10))

    pygame.display.flip() # Update the display

# Clean up and exit the game
pygame.quit()
sys.exit()

