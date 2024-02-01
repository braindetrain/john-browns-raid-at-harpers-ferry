import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the game window
width, height = 460, 307
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Cut the Telephone Line")

# Load images
background = pygame.image.load("background.jpg")
damaged_telephoneline = pygame.image.load("damaged_telephoneline.jpg")

# Set up progress bar variables
progress = 0
progress_speed = 0.1
acceleration = 0.01
max_speed = 5

# Set up progress bar dimensions and color
progress_bar_rect = pygame.Rect(50, height - 50, width - 100, 20)
progress_bar_color = (0, 255, 0)  # Green color

# Set up timer variables
start_time = 0
total_time = 10000  # Reduced total time to 10 seconds in milliseconds

# Set up game state variables
cutting_active = True
required_successes = 5
cut_success_count = 0
pressed_space = False

# Function to draw the telephone line
def draw_telephone_line(image):
    screen.blit(image, (0, 0))
    pygame.display.flip()

# Function to render text
def render_text(text, font, color, position):
    rendered_text = font.render(text, True, color)
    screen.blit(rendered_text, position)

# Display cutscene text
cutscene_font = pygame.font.Font(None, 24)
cutscene_text = [
    "Congratulations! You are now part of the rebellion.",
    "John Brown has given you a simple task:",
    "Restrict communication in the artillery",
    "To do this, you must cut a telephone line",
    "pressing the spacebar to speed up the process",
    "Be nimble and sneaky, as you might get caught."
]

# Set up cutscene state variables
cutscene_active = True
cutscene_frame = 0

# Display cutscene
# Rendering background
screen.blit(pygame.transform.scale(background, (width, height)), (0, 0))
pygame.display.flip()

while cutscene_active:
    pygame.event.pump()  # Process events to prevent freezing

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()

    # Check for spacebar press to skip the cutscene
    if keys[pygame.K_SPACE]:
        cutscene_active = False

    # Rendering cutscene text with a background box
    pygame.draw.rect(screen, (0, 0, 0), (10, 10, width - 20, height - 20))  # Black background box
    for i, line in enumerate(cutscene_text):
        render_text(line, cutscene_font, (255, 255, 255), (20, 20 + i * 40))  # White text
    pygame.display.flip()
    pygame.time.wait(3000)  # Display each line for 3 seconds

    cutscene_frame += 1

    if cutscene_frame == len(cutscene_text):
        cutscene_active = False

# Reset progress bar variables for the main game loop
progress = 0
progress_speed = 0.1
acceleration = 0.01
max_speed = 3

# Reset timer variables for the main game loop
start_time = pygame.time.get_ticks()

# Main game loop
while cutting_active and cut_success_count < required_successes:
    pygame.event.pump()  # Process events to prevent freezing

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()

    # Check for spacebar press to increase acceleration
    if keys[pygame.K_SPACE]:
        acceleration += 0.1
        pressed_space = True  # Add this line to set the pressed_space variable

    # Check for spacebar release
    elif pressed_space:
        pressed_space = False
        progress_speed += acceleration
        progress_speed = min(progress_speed, max_speed)

    # Increase the progress when the spacebar is pressed
    progress += progress_speed

    # Calculate elapsed time
    elapsed_time = pygame.time.get_ticks() - start_time

    # Calculate remaining time
    remaining_time = max(0, total_time - elapsed_time)

    # Rendering background
    screen.blit(pygame.transform.scale(background, (width, height)), (0, 0))

    # Draw the telephone line
    if progress >= progress_bar_rect.width - 1:
        draw_telephone_line(damaged_telephoneline)
        pygame.display.flip()
        pygame.time.delay(500)  # Display damaged telephone line for 500 milliseconds
    else:
        draw_telephone_line(background)

    # Draw the progress bar
    pygame.draw.rect(screen, (0, 0, 0), progress_bar_rect)  # Make progress bar black
    pygame.draw.rect(screen, progress_bar_color, (progress_bar_rect.left, progress_bar_rect.top, progress, progress_bar_rect.height))

    # Draw the timer
    render_text(f"Time Left: {int(remaining_time / 1000)}s", pygame.font.Font(None, 18), (0, 0, 0), (width // 2 - 50, height - 80))

    # If the progress bar is filled, consider it a success
    if progress >= progress_bar_rect.width:
        cut_success_count += 1

        # Set progress bar to green
        progress = progress_bar_rect.width

        # Display damaged telephone line
        draw_telephone_line(damaged_telephoneline)
        pygame.display.flip()
        pygame.time.delay(500)  # Display damaged telephone line for 500 milliseconds

        if cut_success_count >= required_successes:
            cutting_active = False

    # Check if time has run out
    if remaining_time == 0:
        cutting_active = False

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    pygame.time.Clock().tick(30)


# Display success message if the player succeeded
if cut_success_count == 1:
    success_font = pygame.font.Font(None, 16)
    success_text = "Success! You have successfully cut the telephone line and completed the mission."

    # Black box behind white text
    text_surface = success_font.render(success_text, True, (255, 255, 255))
    text_rect = text_surface.get_rect(center=(width // 2, height // 2))
    pygame.draw.rect(screen, (0, 0, 0), text_rect)  # Black box
    screen.blit(text_surface, text_rect)  # White text
    pygame.display.flip()

if cut_success_count < 1:
  failure_font = pygame.font.Font(None, 16)
  failure_text = "Mission Failed! You were caught by an officer and imprisoned."

  # Black box behind white text
  text_surface = failure_font.render(failure_text, True, (255, 255, 255))
  text_rect = text_surface.get_rect(center=(width // 2, height // 2))
  pygame.draw.rect(screen, (0, 0, 0), text_rect)  # Black box
  screen.blit(text_surface, text_rect)  # White text
  pygame.display.flip()


# Wait for a few seconds before quitting
pygame.time.delay(3000)
pygame.quit()
sys.exit()
